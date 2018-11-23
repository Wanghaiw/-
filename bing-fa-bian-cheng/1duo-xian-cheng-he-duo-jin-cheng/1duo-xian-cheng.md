# 线程
在传统操作系统中 , 每个进程有一个地址空间和一个控制线程 , 事实上 , 这几乎就是进程的定义
所以我们可以知道 , 线程是操作系统能够进程运算调度的最小单位 , 它被包含在进程之中 , 是进程中的实际运作单位 . 不过 , 经常存在在同一个地址空间中准并行运行多个控制线程的情况 , 这些线程就像分离的进程
一条线程指的是进程中一个单一顺序的控制流 , 一个进程中可以并发多个线程 , 每条线程并行执行不同的任务。

## Threading
Python通过两个标准库_thread (built-in) 和threading提供对线程的支持 , threading对_thread进行了封装。
threading模块中提供了Thread , Lock , RLock , Semaphore , Event , Condition等组件。

### Thread

参数说明
```
target	表示调用对象 , 即子线程要执行的任务
name	子线程的名称
args	传入target函数中的位置参数 , 是一个元组 , 参数后必须加逗号
kwargs	表示调用对象的字典
```
方法说明
```
Thread.run (self)	进程启动时运行的方法 , 由该方法调用target参数所指定的函数 , 在子类中可以进行重构 , 与线程中一样
Thread.start (self)	启动进程 , start方法就是去帮你调用run方法
Thread.terminate (self)	强制终止线程 , 不会进行任何清理操作 , 使用时需小心其子进程与锁的问题
Thread.join (self, timeout=None)	阻塞调用 , 主线程进行等待 , timeout为超时时间
Thread.is_alive (self)	这个方法在run()方法开始之前返回True , 在run()方法结束之后 , 返回所有活动线程的列表
Thread.isDaemon(self)	判断是否为守护线程 , 返回bool值
Thread.setDaemon(self,daemonic)	将子线程设置为守护线程 , daemonic = daemon
Thread.getName(self,name)	获取线程名称
Thread.setName(self,name)	设置线程名称
```

#### 创建线程
Python中使用线程有两种方式 : 函数或者用类来包装线程对象.

函数调用：
```
import threading
import time
# 定义线程要运行的函数
def func(name):
    print("I am %s" % name)
    # 为了便于观察,让它睡上2秒
    time.sleep(2)
# 防止被导入执行两次
if __name__ == '__main__':
    # 创建一个线程实例,args参数是一个元组,必须加逗号
    t1 = threading.Thread(target=func, args=("Lyon",))
    # 再创建一个线程实例
    t2 = threading.Thread(target=func, args=("Kenneth",))
    # 启动线程
    t1.start()
    # 启动另一个线程
    t2.start()
    # 打印线程名
    print(t1.getName())
    # 打印线程名
    print(t2.getName())
```
类继承调用:
```
import threading
import time
# 继承threading中的Thread类
class MyThread(threading.Thread):
    # 线程中所需要的参数
    def __init__(self, name):
        # threading.Thread.__init__(self)
        super().__init__()
        self.name = name
    # 重构run方法,注意这个是表示线程活动的方法,必须有
    def run(self):
        print("I am %s" % self.name)
        time.sleep(2)
# 防止被导入执行两次
if __name__ == '__main__':
    # 创建一个线程实例
    t1 = MyThread('Lyon')
    # 创建另一个线程实例
    t2 = MyThread('Kenneth')
    # 启动线程,调用了类中的run方法
    t1.start()
    # 启动另一个线程
    t2.start()
    # 获取线程名
    print(t1.getName())
    # 获取线程名
    print(t2.getName())
```

threading模块提供的一些方法：
  * threading.currentThread(): 返回当前的线程变量。
  * threading.enumerate(): 返回一个包含正在运行的线程的list。正在运行指线程启动后、结束前，不包括启动前和终止后的线程。
  * threading.activeCount(): 返回正在运行的线程数量，与len(threading.enumerate())有相同的结果。


 Join & setDaemon 

 Join
 主线程A中，创建了子线程B，并且在主线程A中调用了B.join()，那么，主线程A会在调用的地方等待，直到子线程B完    成操作后，才可以接着往下执行，那么在调用这个线程时可以使用被调用线程的join方法。
 ```
 import threading
import time
def run(name):
    print("I am %s" % name)
    time.sleep(2)
    print("When I'm done, I'm going to keep talking...")
if __name__ == '__main__':
    lyon = threading.Thread(target=run, args=('Lyon',))
    kenneth = threading.Thread(target=run, args=('Kenneth',))
    lyon.start()
    lyon.join()
    kenneth.start()
    kenneth.join()
    print("I was the main thread, and I ended up executing")
 ```

setDaemon

主线程A中，创建了子线程B，并且在主线程A中调用了B.setDaemon(),这个的意思是，把主线程A设置为守护线程，这时候，要是主线程A执行结束了，就不管子线程B是否完成,一并和主线程A退出.
另外需要注意的是setDaemon方法必须在start方法之前调用。
```
import threading
import time
def run(name):
    print("I am %s" % name)
    time.sleep(2)
    print("When I'm done, I'm going to keep talking...")
if __name__ == '__main__':
    lyon = threading.Thread(target=run, args=('Lyon',))
    kenneth = threading.Thread(target=run, args=('Kenneth',))
    # 设置守护线程,必须在启动前设置
    lyon.setDaemon(True)
    # 启动线程
    lyon.start()
    # 设置守护线程
    kenneth.setDaemon(True)
    kenneth.start()
    print("I was the main thread, and I ended up executing")
```

#### GIL
 在CPython解释器中 , 同一个进程下开启的多线程 , 同一时刻只能有一个线程执行 , 无法利用多核优势.
 GIL本质就是一把互斥锁 , 即会将并发运行变成串行 , 以此来控制同一时间内共享数据只能被一个任务进行修改 , 从而保证数据的安全性
 CPython加入GIL主要的原因是为了降低程序的开发复杂度 , 让你不需要关心内存回收的问题 , 你可以理解为Python解释器里有一个独立的线程 , 每过一段时间它起wake up做一次全局轮询看看哪些内存数据是可以被清空的 , 此时你自己的程序 里的线程和Python解释器自己的线程是并发运行的 , 假设你的线程删除了一个变量 , py解释器的垃圾回收线程在清空这个变量的过程中的clearing时刻 , 可能一个其它线程正好又重新给这个还没来及得清空的内存空间赋值了 , 结果就有可能新赋值的数据被删除了 , 为了解决类似的问题 , Python解释器简单粗暴的加了锁 , 即当一个线程运行时 , 其它人都不能动 , 这样就解决了上述的问题 , 这可以说是Python早期版本的遗留问题 . 毕竟Python出来的时候 , 多核处理还没出来呢 , 所以并没有考虑多核问题
以上就可以说明 , Python多线程不适合CPU密集型应用 , 但适用于IO密集型应用.

### 线程之间的同步

#### LOCK
多线程与多进程最大的不同在于 , 多进程中 , 同一个变量 , 各自有一份拷贝存在于每个进程中 , 互不影响 , 但是在多线程中 , 所有变量对于所有线程都是共享的 , 因此 , 线程之间共享数据最大的危险在于多个线程同时修改一个变量 , 那就乱套了 , 所以我们需要GIL一样 , 来锁住数据
上面说了 , 保护不同的数据 , 要加不同的锁 , GIL是为了保护解释器的数据 , 明显我们还需要保护用户数据的锁
所以为了保证用户数据的安全 , 我们需要另一个锁 , 互斥锁(Mutex)。

```
import threading
# 假定这是你的银行存款:
balance = 0
def change_it(n):
    # 先存后取，结果应该为0:
    global balance
    balance = balance + n
    balance = balance - n
# 创建一把锁
lock = threading.Lock()
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
for j in range(10000):
    t1 = threading.Thread(target=run_thread, args=(5,))
    t2 = threading.Thread(target=run_thread, args=(8,))
    t1.start()
    t2.start()
    t1.join()
    
    t2.join()
    print(balance)
    
```

#### 死锁
 所谓死锁 : 是指两个或两个以上的进程或线程在执行过程中 , 因争夺资源而造成的一种互相等待的现象 , 若无外力作用 , 他们都将无法推进下去 . 此时称系统处于死锁状态或系统产生了死锁 , 这些永远在互相等待的进程称为死锁进程 
 在线程间共享多个资源的时候，如果两个线程分别占有一部分资源并且同时等待对方的资源，就会造成死锁。

尽管死锁很少发生，但一旦发生就会造成应用的停止响应。下面看一个死锁的例子
```
import threading
import time

class MyThread1(threading.Thread):
    def run(self):
        if mutexA.acquire():
            print(self.name+'----do1---up----')
            time.sleep(1)

            if mutexB.acquire():
                print(self.name+'----do1---down----')
                mutexB.release()
            mutexA.release()

class MyThread2(threading.Thread):
    def run(self):
        if mutexB.acquire():
            print(self.name+'----do2---up----')
            time.sleep(1)
            if mutexA.acquire():
                print(self.name+'----do2---down----')
                mutexA.release()
            mutexB.release()

mutexA = threading.Lock()
mutexB = threading.Lock()

if __name__ == '__main__':
    t1 = MyThread1()
    t2 = MyThread2()
    t1.start()
    t2.start()
    
```

#### RLOCK
RLock叫做递归锁，在Python中为了支持在同一线程中多次请求同一资源 , Python提供了可重入锁RLock

这个RLock内部维护着一个Lokc和一个counter变量 , counter记录了acquire的次数 , 从而使得资源可以被多次require . 知道一个线程所有的acquire都被release , 其他的线程才能获得资源。
对上面lock的例子做如下修改
```
# 仅仅只需如下修改
mutexA = threading.Lock()
mutexB = threading.Lock()
# 以上两行修改为
mutexA = mutexB = threading.RLock()
# 注意如果仅仅修改后部分,即将Lock() -> RLock()是不行的,那样等于创建了两把递归锁

```

####  Condition

条件同步机制是指：一个线程等待特定条件，而另一个线程发出特定条件满足的信号。 解释条件同步机制的一个很好的例子就是生产者/消费者（producer/consumer）模型。生产者随机的往列表中“生产”一个随机整数，而消费者从列表中“消费”整数
在producer类中，producer获得锁，生产一个随机整数，通知消费者有了可用的“商品”，并且释放锁。producer无限地向列表中添加整数，同时在两个添加操作中间随机的停顿一会儿。
``` 
class Producer(threading.Thread):
"""
向列表中生产随机整数
"""
def __init__(self, integers, condition):
    """
    构造器
    @param integers 整数列表
    @param condition 条件同步对象
    """
    threading.Thread.__init__(self)
    self.integers = integers
    self.condition = condition
def run(self):
    """
    实现Thread的run方法。在随机时间向列表中添加一个随机整数
    """
    while True:
        integer = random.randint(0, 256)
        self.condition.acquire()    #获取条件锁
        print('线程%s,获取了锁' % self.name)
        self.integers.append(integer)
        print('%d 添加到了list里面 %s' % (integer, self.name))
        print('condition notified by %s' % self.name)
        self.condition.notify() #唤醒消费者线程
        print(' 线程%s 释放了锁' % self.name)
        self.condition.release()    #释放条件锁
        time.sleep(1)       #暂停1秒钟
        
```
下面是消费者（consumer）类。它获取锁，检查列表中是否有整数，如果没有，等待生产者的通知。当消费者获取整数之后，释放锁。 
注意在wait()方法中会释放锁，这样生产者就能获得资源并且生产“商品”。
```
class Consumer(threading.Thread):
"""
从列表中消费整数
"""
def __init__(self, integers, condition):
    """
    构造器
    @param integers 整数列表
    @param condition 条件同步对象
    """
    threading.Thread.__init__(self)
    self.integers = integers
    self.condition = condition
def run(self):
    """
    实现Thread的run()方法，从列表中消费整数
    """
    while True:
        self.condition.acquire()    #获取条件锁
        print('线程 %s获取了锁' % self.name)
        while True:
            if self.integers:   #判断是否有整数
                integer = self.integers.pop()
                print('%d 从list里面删除了 %s' % (integer, self.name))
                break
        print('释放锁,等待生产者生产数据%s' % self.name)
        self.condition.wait()   #等待商品，并且释放资源
        print('线程 %s 释放锁' % self.name)
        self.condition.release()    #最后释放条件锁
        
```
#### Event
Event对象中包含一个可由线程设置的信号标志 , 它允许线程等待某些事件的发生 . 在初始情况下 , Event对象中的信号标志被设置为假 ; 如果有线程等待一个Event对象 , 而这个Event对象的标志为假 , 那么这个线程将会被一直阻塞直至该标志为真 . 一个线程如果将一个Event对象的信号标志设置为真 , 它将唤醒所有等待这个Event对象的线程 . 如果一个线程等待一个已经被设置为真的Event对象 , 那么它将忽略这个事件 , 继续执行.
基于事件的同步是指：一个线程发送/传递事件，另外的线程等待事件的触发。 让我们再来看看前面的生产者和消费者的例子，现在我们把它转换成使用事件同步而不是条件同步。 
首先是生产者类，我们传入一个Event实例给构造器而不是Condition实例。一旦整数被添加进列表，事件(event)被设置和发送去唤醒消费者。注意事件(event)实例默认是被发送的。
```
class Producer(threading.Thread):
  """
  向列表中生产随机整数
  """
  def __init__(self, integers, event):
    """
    构造器
    @param integers 整数列表
    @param event 事件同步对象
    """
    threading.Thread.__init__(self)
    self.integers = integers
    self.event = event
  def run(self):
    """
    实现Thread的run方法。在随机时间向列表中添加一个随机整数
    """
    while True:
      integer = random.randint(0, 256)
      self.integers.append(integer)
      print '%d appended to list by %s' % (integer, self.name)
      print 'event set by %s' % self.name
      self.event.set()      #设置事件   
      self.event.clear()    #发送事件
      print 'event cleared by %s' % self.name
      time.sleep(1)
      
```

 同样我们传入一个Event实例给消费者的构造器，消费者阻塞在wait()方法，等待事件被触发，即有可供消费的整数。
 ```
 class Consumer(threading.Thread):
  """
   从列表中消费整数
  """
  def __init__(self, integers, event):
    """
    构造器
    @param integers 整数列表
    @param event 事件同步对象
    """
    threading.Thread.__init__(self)
    self.integers = integers
    self.event = event
  def run(self):
    """
    实现Thread的run()方法，从列表中消费整数
    """
    while True:
      self.event.wait() #等待事件被触发
      try:
        integer = self.integers.pop()
        print '%d popped from list by %s' % (integer, self.name)
      except IndexError:
        # catch pop on empty list
        time.sleep(1)
 ```
Event的基本方法
```
Event.isSet()	返回Event的状态 , isSet == is_set
Event.wait()	如果Event.isSet() == False将阻塞线程
Event.set()	设置Event的状态值为True , 所有阻塞池中的线程激活进入就绪状态 , 等待操作系统调度
Event.clear()	回复Event的状态值为False
```
####  Queue(队列)

Queue - 一种线程安全的FIFO实现 
Python的Queue模块提供一种适用于多线程编程的FIFO实现。它可用于在生产者(producer)和消费者(consumer)之间线程安全(thread-safe)地传递消息或其它数据，因此多个线程可以共用同一个Queue实例。Queue的大小（元素的个数）可用来限制内存的使用。
队列是一个非常好的线程同步机制，使用队列我们不用关心锁，队列会为我们处理锁的问题。 队列(Queue)有以下4个常用的方法：
```
Queue.Queue(maxsize=0)   FIFO， 如果maxsize小于1就表示队列长度无限
Queue.LifoQueue(maxsize=0)   LIFO， 如果maxsize小于1就表示队列长度无限
Queue.qsize()   返回队列的大小 
Queue.empty()   如果队列为空，返回True,反之False 
Queue.full()   如果队列满了，返回True,反之False
Queue.get([block[, timeout]])    读队列，timeout等待时间 
Queue.put(item, [block[, timeout]])   写队列，timeout等待时间 
Queue.queue.clear()   清空队列
Queue.join: 阻塞知道所有的项目都被处理完。
Queue.task_done: 当某一项任务完成时调用；
```
下面我们将上面的生产者/消费者的例子转换成使用队列。 
首先是生产者类，我们不需要传入一个整数列表，因为我们使用队列就可以存储生成的整数。生产者线程在一个无限循环中生成整数并将生成的整数添加到队列中。
```
class Producer(threading.Thread):
  """
  向队列中生产随机整数
  """
  def __init__(self, queue):
    """
    构造器
    @param integers 整数列表    #译注：不需要这个参数
    @param queue 队列同步对象
    """
    threading.Thread.__init__(self)
    self.queue = queue
  def run(self):
    """
    实现Thread的run方法。在随机时间向队列中添加一个随机整数
    """
    while True:
      integer = random.randint(0, 256)
      self.queue.put(integer)   #将生成的整数添加到队列
      print '%d put to queue by %s' % (integer, self.name)
      time.sleep(1)
```

  下面是消费者类。线程从队列中获取整数，并且在任务完成时调用task_done()方法。
  ```
  class Consumer(threading.Thread):
  """
  从队列中消费整数
  """
  def __init__(self, queue):
    """
    构造器
    @param integers 整数列表    #译注：不需要这个参数
    @param queue 队列同步对象
    """
    threading.Thread.__init__(self)
    self.queue = queue
  def run(self):
    """
    实现Thread的run()方法，从队列中消费整数
    """
    while True:
      integer = self.queue.get()
      print '%d popped from list by %s' % (integer, self.name)
      self.queue.task_done()
  
  ```
 队列同步的最大好处就是队列帮我们处理了锁。在python内部队列（Queue）构造器创建一个锁，保护队列元素的添加和删除操作。同时创建了一些条件锁对象处理队列事件，比如队列不空事件（削除get()的阻塞），队列不满事件（削除put()的阻塞）和所有项目都被处理完事件（削除join()的阻塞）。

  

 ### 线程池
 线程池是预先创建线程的一种技术。线程池在还没有任务到来之前，创建一定数量的线程，放入空闲队列中。这些线程都是处于睡眠状态，即均为启动，不消耗CPU，而只是占用较小的内存空间。当请求到来之后，缓冲池给这次请求分配一个空闲线程，把请求传入此线程中运行，进行处理。当预先创建的线程都处于运行状态，即预制线程不够，线程池可以自由创建一定数量的新线程，用于处理更多的请求。当系统比较闲的时候，也可以通过移除一部分一直处于停用状态的线程。
一个典型的线程池，应该包括如下几个部分： 
1、线程池管理器（ThreadPool），用于启动、停用，管理线程池 
2、工作线程（WorkThread），线程池中的线程 
3、请求接口（WorkRequest），创建请求对象，以供工作线程调度任务的执行 
4、请求队列（RequestQueue）,用于存放和提取请求 
5、结果队列（ResultQueue）,用于存储请求执行后返回的结果


##### multiprocessing.dummy
multiprocessing.dummy 模块与 multiprocessing 模块的区别： dummy 模块是多线程，而 multiprocessing 是多进程， api 都是通用的。 所有可以很方便将代码在多线程和多进程之间切换例子：

例子：
```
from multiprocessing.dummy import Pool as ThreadPool
import time
import urllib.request
urls = [
    'http://www.baidu.com',
    'http://tieba.baidu.com/',
    'http://www.qq.com',
    'http://www.youku.com',
    'http://www.tudou.com'
]
start = time.time()
result = map(urllib.request.urlopen,urls)
print('Normal:', time.time() - start)
start2 = time.time()
#processes默认是 cpu 的核心数
pool = ThreadPool(processes=4)
results2 = pool.map(urllib.request.urlopen, urls)
pool.close()
pool.join()
print('Thread Pool:', time.time() - start2)
```


* 作业 通过使用threading和queue自己写一个线程池。

```
import time
import threading
from random import random
from queue import Queue
def double(n):
    return n * 2
class Worker(threading.Thread):
    def __init__(self, queue):
        super(Worker, self).__init__()
        self._q = queue
        self.daemon = True
        self.start()
    def run(self):
        print('启动')
        while True:
            f, args, kwargs = self._q.get()
            try:
                print('USE: {}'.format(self.name))  # 线程名字
                print(f(*args, **kwargs))
                time.sleep(2)
            except Exception as e:
                print(e)
            self._q.task_done()
class ThreadPool(object):
    def __init__(self, num_t=10):
        self._q = Queue(num_t)
        # Create Worker Thread
        for _ in range(num_t):
            Worker(self._q)
    def add_task(self, f, *args, **kwargs):
        self._q.put((f, args, kwargs))
    def wait_complete(self):
        self._q.join()
pool = ThreadPool()
for _ in range(30000):
    wt = random()
    pool.add_task(double, wt)
    #time.sleep(wt)
pool.wait_complete()
```

