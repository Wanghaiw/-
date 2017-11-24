# 进程
进程，就是这种“程序切换”的第一种方式。

## 定义
进程，是执行中的计算机程序。也就是说，每个代码在执行的时候，首先本身即是一个进程。

一个进程具有：就绪，运行，中断，僵死，结束等状态（不同操作系统不一样）。
## 使用
用户编写代码(代码本身是以进程运行的)
启动程序，进入进程“就绪”状态
操作系统调度资源，做“程序切换”，使得进程进入“运行”状态
结束/中断
程序执行完，则进入“结束”状态
程序未执行完，但操作系统达到“程序切换”的要求，进入“中断”状态，等待下次被调度后执行

## 特性
* 每个程序，本身首先是一个进程
* 运行中每个进程都拥有自己的地址空间、内存、数据栈及其它资源。
* 操作系统本身自动管理着所有的进程(不需要用户代码干涉)，并为这些进程合理分配可以执行时间。
* 进程可以通过派生新的进程来执行其它任务，不过每个进程还是都拥有自己的内存和数据栈等。
* 进程间可以通讯(发消息和数据)，采用 进程间通信(IPC) 方式。
* 多个进程可以在不同的 CPU 上运行，互不干扰
* 同一个CPU上，可以运行多个进程，由操作系统来自动分配时间片
* 由于进程间资源不能共享，需要进程间通信，来发送数据，接受消息等

多进程，也称为“并行”。

# Python 中的进程
在 Python 中，通过 Python 解释器执行的任何代码，首先本身都是一个进程，比如：
``` -$ python test.py ```

Python 解释器会自动启动一个进程，并加载 test.py 中的代码执行。
我们这里研究的进程是：如何在这样的过程中，创建另一个附加进程，或者说如何创建“子进程”。也就是想办法“手动”创建一个进程，而不是自动生成。
只有能创建“子进程”了，我们才能手动编些进程来执行额外的代码，而不是Python 自动创建，自动执行，有助于我们合理和灵活的利用多 CPU 的情形。

## 创建子进程

#### fork()
在Unix/Linux操作系统提供了一个fork()函数，它非常特殊，调用一次，返回两次，因为操作系统将当前的进程（父进程）复制了一份（子进程），然后分别在父进程和子进程内返回。windows不支持fork方法。
```
import os
pid = os.fork()
if pid == 0:
    print('子进程')
else:
    print('父进程')
    ```
说明:
* 程序执行到`os.fork()`的时，操作系统会创建一个新的进程(子进程)，然后复制父进程的所有信息到子进程。
* `os.fork()`的返回值有两个，子进程返回0，父进程返回子进程的id，这样做的理由是，一个父进程可以fork()出很多子进程，所以，父进程要记下每个子进程的ID，而子进程只需要调用getppid()就可以拿到父进程的ID。

#### getpid() getppid()

```
import os

pid = os.fork()
if pid == 0:
    print('我是子进程{},我的父进程是{}'.format(os.getpid(),os.getppid()))
else:
    print('我是父进程{},我的子进程是{}'.format(os.getpid(),pid))
```

* 思考： 当出现多次fork时，进程的变化 

#### multiprocessing

由于Python是跨平台的，自然也应该提供一个跨平台的多进程支持。multiprocessing模块就是跨平台版本的多进程模块。
multiprocessing模块提供了一个Process类来代表一个进程对象，下面的例子演示了启动一个子进程并等待其结束：
```
from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('子进程运行中，name= %s ,pid=%d...' % (name, os.getpid()))

if __name__=='__main__':
    print('父进程 %d.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('子进程将要执行')
    p.start()
    p.join()
    print('子进程已结束')
```
说明:
* 创建子进程时，只需要传入一个执行函数和函数的参数，创建一个Process实例，用start()方法启动，这样创建进程比fork()还要简单。
* join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步。

Process语法结构如下：
    
    Process([group [, target [, name [, args [, kwargs]]]]])
    target：表示这个进程实例所调用对象
    args：表示调用对象的位置参数元组
    kwargs：表示调用对象的关键字参数字典
    name：为当前进程实例的别名
    group：进程所属组，大多数情况下用不到
    
Process类常用方法：
    
    current_process():获当前线程的名字
    is_alive()：判断进程实例是否还在执行
    join([timeout])：是否等待进程实例执行结束，或等待多少秒
    start()：启动进程实例（创建子进程）
    run()：如果没有给定target参数，对这个对象调用start()方法时，就将执行对象中的run()方法
    terminate()：不管任务是否完成，立即终止

Process类常用属性：

    name：当前进程实例别名，默认为Process-N，N为从1开始递增的整数
    pid：当前进程实例的PID值
    daemon：置此线程是否被主线程守护回收。默认False不回收，需要在 start 方法前调用；设为True相当于像主线程中注册守护，主线程结束时会将其一并回收
    exitcode：判断进程的运行状态，进程在运行时为None
    

########## demo1
```
from multiprocessing import Process
import os
from time import sleep

def run_proc(name, age, **kwargs):
    for i in range(10):
        print('子进程运行中，name= {},age={} ,pid={}...'.format(name, age,os.getpid()))
        print(kwargs)
        sleep(0.5)

if __name__=='__main__':
    print('父进程{}'.format(os.getpid()))
    p = Process(target=run_proc, args=('test',18), kwargs={"m":20})
    print('子进程将要执行')
    p.start()
    sleep(1)
    p.terminate()
    p.join()
    print('子进程已结束')

```

######### demo2
```
from multiprocessing import Process
import time
import os

#两个子进程将会调用的两个方法
def  worker_1(interval):
    print("worker_1,父进程(%s),当前进程(%s)"%(os.getppid(),os.getpid()))
    t_start = time.time()
    time.sleep(interval) #程序将会被挂起interval秒
    t_end = time.time()
    print("worker_1,执行时间为'%0.2f'秒"%(t_end - t_start))

def  worker_2(interval):
    print("worker_2,父进程(%s),当前进程(%s)"%(os.getppid(),os.getpid()))
    t_start = time.time()
    time.sleep(interval)
    t_end = time.time()
    print("worker_2,执行时间为'%0.2f'秒"%(t_end - t_start))

#输出当前程序的ID
print("进程ID：%s"%os.getpid())

#创建两个进程对象，target指向这个进程对象要执行的对象名称，
#如果不指定name参数，默认的进程对象名称为Process-N，N为一个递增的整数
p1=Process(target=worker_1,args=(2,))
p2=Process(target=worker_2,name="dongGe",args=(1,))

p1.start()
p2.start()

print("p2.is_alive=%s"%p2.is_alive())

print("p1.name=%s"%p1.name)
print("p1.pid=%s"%p1.pid)
print("p2.name=%s"%p2.name)
print("p2.pid=%s"%p2.pid)
p1.join()
print("p1.is_alive=%s"%p1.is_alive())

```
## 进程的创建-Process子类
创建新的进程还能够使用类的方式，可以自定义一个类，继承Process类，每次实例化这个类的时候，就等同于实例化一个进程对象，请看下面的实例：
```
import multiprocessing
import time
class ClockProcess(multiprocessing.Process):
    def __init__(self, interval):
        multiprocessing.Process.__init__(self)
        self.interval = interval
    def run(self):
        n = 5
        while n > 0:
            print("the time is {0}".format(time.ctime()))
            time.sleep(self.interval)
            n -= 1
if __name__ == '__main__':
    for i in range(5):
        p = ClockProcess(3)
        p.start()
```

## 进程池

当需要创建的子进程数量不多时，可以直接利用multiprocessing中的Process动态成生多个进程，但如果是上百甚至上千个目标，手动的去创建进程的开销巨大，此时就可以用到multiprocessing模块提供的Pool方法。

初始化Pool时，可以指定一个最大进程数，当有新的请求提交到Pool中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求；但如果池中的进程数已经达到指定的最大值，那么该请求就会等待，直到池中有进程结束，才会创建新的进程来执行，请看下面的实例：
```
from multiprocessing import Pool
import os,time,random

def worker(msg):
    t_start = time.time()
    print("%s开始执行,进程号为%d"%(msg,os.getpid()))
    #random.random()随机生成0~1之间的浮点数
    time.sleep(random.random()*2)
    t_stop = time.time()
    print(msg,"执行完毕，耗时%0.2f"%(t_stop-t_start))

po=Pool(3) #定义一个进程池，最大进程数3
for i in range(0,10):
    #Pool.apply_async(要调用的目标,(传递给目标的参数元祖,))
    #每次循环将会用空闲出来的子进程去调用目标
    po.apply_async(worker,(i,))

print("----start----")
po.close() #关闭进程池，关闭后po不再接收新的请求
po.join() #等待po中所有子进程执行完成，必须放在close语句之后
print("-----end-----")
```
multiprocessing.pool 常用函数解析：
* apply(func[, args=()[, kwds={}]]) 该函数用于传递不定参数，主进程会被阻塞直到函数执行结束（不建议使用，并且3.x以后不在出现）。
* apply_async(func[, args=()[, kwds={}[, callback=None]]])
与apply用法一样，但它是非阻塞且支持结果返回进行回调。
* map(func, iterable[, chunksize=None])
Pool类中的map方法，与内置的map函数用法行为基本一致，它会使进程阻塞直到返回结果。 
注意，虽然第二个参数是一个迭代器，但在实际使用中，必须在整个队列都就绪后，程序才会运行子进程。
* close()
关闭进程池（pool），使其不在接受新的任务。
* terminate(), 不管任务是否完成，立即结束任务。
* join()
主进程阻塞等待子进程的退出，join方法必须在close或terminate之后使用。

####  apply堵塞式和apply_async非堵塞式
```
from multiprocessing import Pool
import os,time,random

def worker(msg):
    t_start = time.time()
    print("%s开始执行,进程号为%d"%(msg,os.getpid()))
    #random.random()随机生成0~1之间的浮点数
    time.sleep(random.random()*2)
    t_stop = time.time()
    print(msg,"执行完毕，耗时%0.2f"%(t_stop-t_start))

po=Pool(3) #定义一个进程池，最大进程数3
for i in range(0,10):
    po.apply(worker,(i,))

print("----start----")
po.close() #关闭进程池，关闭后po不再接收新的请求
po.join() #等待po中所有子进程执行完成，必须放在close语句之后
print("-----end-----")
```

* 尝试把apply改成apply_async 对比运行结果，并说明原因。
* 自行尝试map方法的作用。

## 进程间的同步

### Lock
进程之间的数据是不共享的 , 因为每个进程之间是相互独立的 , 但是进程共享一套文件系统 , 所以访问同一个文件 , 是没有问题的 , 但是如果有多个进程对同一文件进行修改 , 就会造成错乱 , 所以我们为了保护文件数据的安全 , 就需要给其进行加锁。
例子：
```
import multiprocessing
# 假定这是你的银行存款:
balance = 0
def change_it(n):
    # 先存后取，结果应该为0:
    global balance
    balance = balance + n
    balance = balance - n
# 创建一把锁
lock = multiprocessing.Lock()
def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
# 在多线程例子中并没有写这句,但是多进程中使用start()必须加
if __name__ == '__main__':
    for j in range(10000):
        t1 = multiprocessing.Process(target=run_thread, args=(5,))
        t2 = multiprocessing.Process(target=run_thread, args=(8,))
        t1.start()
        t2.start()
        t1.join()
        t2.join()
        print(balance)
        
```

#### Producer-consumer 生产者和消费者模型
```
import time
import random
import multiprocessing
q = multiprocessing.Queue()

def Producer(name, q):
    count = 1
    while count < 5:
        time.sleep(random.randrange(3))
        q.put(count)
        print('Producer %s has produced %s bun...' % (name, count))
        count += 1

def Consumer(name , q):
    count = 1
    while count < 20:
        time.sleep(random.randrange(4))
        if not q.empty():
            data = q.get()
            print(data)
            print('\033[32;1mConsumer %s has eat %s bun...\033[0m' % (name, data))
        else:
            print("No bun anymore...")


if __name__ == '__main__':
    # 进程间的数据是不共享的,注意我们需要把q,即队列对象传入函数中
    p1 = multiprocessing.Process(target=Producer, args=('Lyon', q,))
    c1 = multiprocessing.Process(target=Consumer, args=('Kenneth', q,))
    p1.start()
    c1.start()
    p1.join()
    c1.join()
    print("End of main process...")
    
```

### Semaphore 
信号量semaphore 
是一个变量，控制着对公共资源或者临界区的访问。信号量维护着一个计数器，指定可同时访问资源或者进入临界区的线程数。 
每次有一个线程获得信号量时，计数器-1。若计数器为0，其他线程就停止访问信号量，直到另一个线程释放信号量。 
信号量同步的例子：




## 进程间的通信-Queue
Queue是多进程安全的队列，可以使用multiprocessing 里面的
Queue实现多进程之间的数据传递。Queue本身是一个消息队列程序。
接下来看一个小demo来演示Queue的使用方法：
```
from multiprocessing import Queue
q=Queue(3) #初始化一个Queue对象，最多可接收三条put消息
q.put("消息1")
q.put("消息2")
print(q.full())  #False
q.put("消息3")
print(q.full()) #True

#因为消息列队已满下面的try都会抛出异常，第一个try会等待2秒后再抛出异常，第二个Try会立刻抛出异常
try:
    q.put("消息4",True,2)
except:
    print("消息列队已满，现有消息数量:%s"%q.qsize())

try:
    q.put_nowait("消息4")
except:
    print("消息列队已满，现有消息数量:%s"%q.qsize())

#推荐的方式，先判断消息列队是否已满，再写入
if not q.full():
    q.put_nowait("消息4")

#读取消息时，先判断消息列队是否为空，再读取
if not q.empty():
    for i in range(q.qsize()):
        print(q.get_nowait())
```

说明：
初始化Queue()对象时（例如：q=Queue()），若括号中没有指定最大可接收的消息数量，或数量为负值，那么就代表可接受的消息数量没有上限（直到内存的尽头）；

    Queue.qsize()：返回当前队列包含的消息数量；
    
    Queue.empty()：如果队列为空，返回True，反之False ；
    
    Queue.full()：如果队列满了，返回True,反之False；

    Queue.get([block[, timeout]])：获取队列中的一条消息，然后将其从列队中移除，block默认值为True；

* 如果block使用默认值，且没有设置timeout（单位秒），消息列队如果为空，此时程序将被阻塞（停在读取状态），直到从消息列队读到消息为止，如果设置了timeout，则会等待timeout秒，若还没读取到任何消息，则抛出"Queue.Empty"异常；

* 如果block值为False，消息列队如果为空，则会立刻抛出"Queue.Empty"异常；

    Queue.get_nowait()：相当Queue.get(False)；

    Queue.put(item,[block[, timeout]])：将item消息写入队列，block默认值为True；

* 如果block使用默认值，且没有设置timeout（单位秒），消息列队如果已经没有空间可写入，此时程序将被阻塞（停在写入状态），直到从消息列队腾出空间为止，如果设置了timeout，则会等待timeout秒，若还没空间，则抛出"Queue.Full"异常；

* 如果block值为False，消息列队如果没有空间可写入，则会立刻抛出"Queue.Full"异常；

    Queue.put_nowait(item)：相当Queue.put(item, False)；
    
  
我们以Queue为例，在父进程中创建两个子进程，一个往Queue里写数据，一个从Queue里读数据：
```
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    for value in range(10):
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    while True:
        if not q.empty():
            value = q.get(True)
            print('Get %s from queue.' % value)
            time.sleep(random.random())

        else:
            break

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue(10)
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    pw.start()
    pw.join()
    pr.start()
    pr.join()
    print('所有数据都写入并且读完')
    
```
### Manager 
进程之间是相互独立的 , 在multiprocessing模块中的Manager可以实现进程间数据共享 , 并且Manager还支持进程中的很多操作 , 比如Condition , Lock , Namespace , Queue , RLock , Semaphore等.
demo：
```
import multiprocessing
# 既然数据共享了,就需要像多线程那样,防止竞争
def run(d,lock):
      # 演示没加锁的实例
    # lock.acquire()
    d['count'] -= 1
    # lock.release()
if __name__ == '__main__':
    # lock = multiprocessing.Lock()
    with multiprocessing.Manager() as m:
        dic = m.dict({'count' : 100})
        process_list = []
        for i in range(100):
            p = multiprocessing.Process(target=run, args=(dic, lock,))
            process_list.append(p)
            p.start()
        for p in process_list:
            p.join()
        print(dic)
        
```




作业：多线程copy文件

```
from multiprocessing import Pool,Manager
import os, time


def copyFileTask(filename, oldFolderName, newFolderName, queue):
    with open(oldFolderName+"/"+filename,encoding='utf-8',errors='ignore') as fr:
        content = fr.read()
    with open(newFolderName+"/"+filename, 'w',encoding='utf-8') as fw:
        fw.write(content)
    queue.put(filename)

def main():
    oldFolderName = input('输入您要复制的文件夹:')
    newFolderName = oldFolderName + '复制'
    not os.path.exists(newFolderName) and os.mkdir(newFolderName)
    filename = os.listdir(oldFolderName)
    queue = Manager().Queue()

    pool = Pool(1)

    for name in filename:
        pool.apply_async(func=copyFileTask, args=(name, oldFolderName, newFolderName, queue))



    num = 0
    total = len(filename)
    print('总文件数是:{}'.format(total))
    while num < total:
        queue.get()
        num += 1
        print('\r当前复制进度%.2f%%, num是%d'%(((num/total)*100), num),end='')

    #print('已完成拷贝copy......')


if __name__ == '__main__':
    start_time = time.time()
    main()
    print('\n')
    print('完成copy用时{}秒.....'.format(time.time() -start_time))
    
```





j


