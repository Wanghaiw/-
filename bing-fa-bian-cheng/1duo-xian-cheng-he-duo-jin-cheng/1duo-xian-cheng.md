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
创建线程
Python中使用线程有两种方式 : 函数或者用类来包装线程对象