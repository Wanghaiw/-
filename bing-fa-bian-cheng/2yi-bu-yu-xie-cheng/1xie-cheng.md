
# 协程

## 什么是协程
协程，又称微线程，纤程，英文名Coroutine。协程的作用，是在执行函数A时，可以随时中断，去执行函数B，然后中断继续执行函数A（可以自由切换）。但这一过程并不是函数调用（没有调用语句），这一整个过程看似像多线程，然而协程只有一个线程执行。

协程 是为非抢占式多任务产生子程序的计算机程序组件，协程允许不同入口点在不同位置暂停或开始执行程序”。从技术的角度来说，“协程就是你可以暂停执行的函数”。如果你把它理解成“就像生成器一样”，那么你就想对了。

## 优点
* 1.执行效率极高，因为子程序切换（函数）不是线程切换，由程序自身控制，没有切换线程的开销。所以与多线程相比，线程的数量越多，协程性能的优势越明显。
* 不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在控制共享资源时也不需要加锁，因此执行效率高很多。

# python2.X的协程

## 主要应用：
yield和gevent
python2.x中支持协程的模块不多，常用的就是gevent。我们主要讲解gevent的用法。

## Gevent
gevent是第三方库，通过greenlet实现协程，其基本思想： 当一个greenlet遇到IO操作时，比如访问网络，就自动切换到其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证总有greenlet在运行，而不是等待IO。

gevent实现了python标准库里面大部分的阻塞式系统调用，包括socket、ssl、threading和select等模块，而将这些阻塞式调用变为协作式运行。通过monkey patch修改Python自带的一些标准库。
例子:
```
from gevent import monkey;monkey.patch_all() #修改标准库
import gevent
import requests

def get_response(pn):
    print('start',pn)
    requests.get('https://www.baidu.com')
    print('end',pn)

tasks = [gevent.spawn(get_response,pn) for pn in range(5)]
gevent.joinall(tasks)
```
Gevent的方法:
```
monkey使一些阻塞的模块变得不阻塞，机制：遇到IO操作则自动切换，手动切换可以用gevent.sleep(0)（将爬虫代码换成这个，效果一样可以达到切换上下文）
gevent.spawn 启动协程，参数为函数名称，参数名称
gevent.spawn()”方法会创建一个新的greenlet协程对象，并运行它。”gevent.joinall()”方法会等待所有传入的greenlet协程运行结束后再退出，这个方法可以接受一个”timeout”参数来设置超时时间，单位是秒。
gevent.joinall 阻塞当前流程，并执行所有给定的greenlet。执行流程只会在所有greenlet执行完后才会继续向下走。
```

# python3.x的协程
Python对协程的支持是通过generator实现的。
在generator中，我们不但可以通过for循环来迭代，还可以不断调用next()函数获取由yield语句返回的下一个值。
但是Python的yield不但可以返回一个值，它还可以接收调用者发出的参数。
来看例子：
```
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)  #启动生成器,没有返回值
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)
```
```
注意到consumer函数是一个generator，把一个consumer传入produce后：
首先调用c.send(None)启动生成器；
然后，一旦生产了东西，通过c.send(n)切换到consumer执行；
consumer通过yield拿到消息，处理，又通过yield把结果传回；
produce拿到consumer处理的结果，继续生产下一条消息；
produce决定不生产了，通过c.close()关闭consumer，整个过程结束。
整个流程无锁，由一个线程执行，produce和consumer协作完成任务，所以称为“协程”，而非线程的抢占式多任务。
```


上面只是协程的简单操作，python3中对协程的编程模型引入了一些标准库.
Python3.4以后引入了asyncio模块，可以很好的支持协程.
asyncio的编程模型就是一个消息循环。我们从asyncio模块中直接获取一个EventLoop的引用，然后把需要执行的协程扔到EventLoop中执行，就实现了异步IO。

```
import asyncio
@asyncio.coroutine
def hello():
    print("Hello world!")
    # 异步调用asyncio.sleep(1):
    r = yield from asyncio.sleep(2)
    print("Hello again!")
# 获取EventLoop:
loop = asyncio.get_event_loop()
# 执行coroutine
loop.run_until_complete(hello())
loop.close()
```

asyncio说明

    @asyncio.coroutine把一个generator标记为coroutine类型，然后，我们就把这个coroutine扔到EventLoop中执行。
    hello()会首先打印出Hello world!，然后，yield from语法可以让我们方便地调用另一个generator。由于asyncio.sleep()也是一个coroutine，所以线程不会等待asyncio.sleep()，而是直接中断并执行下一个消息循环。当asyncio.sleep()返回时，线程就可以从yield from拿到返回值（此处是None），然后接着执行下一行语句。
    把asyncio.sleep(1)看成是一个耗时1秒的IO操作，在此期间，主线程并未等待，而是去执行EventLoop中其他可以执行的coroutine了，因此可以实现并发执行。
    
我们可以用Task封装两个coroutine试试：
```
import threading
import asyncio

@asyncio.coroutine
def hello():
    print('Hello world! (%s)' % threading.currentThread())
    yield from asyncio.sleep(1)
    print('Hello again! (%s)' % threading.currentThread())

loop = asyncio.get_event_loop()
tasks = [hello(), hello()]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()

由打印的当前线程名称可以看出，两个coroutine是由同一个线程并发执行的。
```

为了简化并更好地标识异步IO，从Python 3.5开始引入了新的语法async和await，可以让coroutine的代码更简洁易读。
请注意，async和await是针对coroutine的新语法，要使用新的语法，只需要做两步简单的替换：

    把@asyncio.coroutine替换为async；
    把yield from替换为await。
    
例如：
```    
import asyncio
async def test(i):
	print("test_1",i)
	await asyncio.sleep(1)
	print("test_2",i)
loop=asyncio.get_event_loop()
tasks=[test(i) for i in range(5)]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()
```
