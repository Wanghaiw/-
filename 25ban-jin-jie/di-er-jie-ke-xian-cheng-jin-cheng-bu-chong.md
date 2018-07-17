### 为什么要使用python多进程？

　　因为python使用全局解释器锁(GIL)，他会将进程中的线程序列化，也就是多核cpu实际上并不能达到并行提高速度的目的，而使用多进程则是不受限的，所以实际应用中都是推荐多进程的。
　　如果每个子进程执行需要消耗的时间非常短（执行+1操作等），这不必使用多进程，因为进程的启动关闭也会耗费资源。
　　当然使用多进程往往是用来处理CPU密集型（科学计算）的需求，如果是IO密集型（文件读取，爬虫等）则可以使用多线程去处理。

### multiprocessing组件Process
```
构造方法：
Process([group [, target [, name [, args [, kwargs]]]]])
group: 线程组，目前还没有实现，库引用中提示必须是None；
target: 要执行的方法；
name: 进程名；
args/kwargs: 要传入方法的参数。

实例方法：
is_alive()：返回进程是否在运行。
join([timeout])：阻塞当前上下文环境的进程程，直到调用此方法的进程终止或到达指定的timeout（可选参数）。
start()：进程准备就绪，等待CPU调度。
run()：strat()调用run方法，如果实例进程时未制定传入target，这时start执行默认run()方法。
terminate()：不管任务是否完成，立即停止工作进程。

属性：
authkey
daemon：和线程的setDeamon功能一样（将父进程设置为守护进程，当父进程结束时，子进程也结束）。
exitcode(进程在运行时为None、如果为–N，表示被信号N结束）。
name：进程名字。
pid：进程号。
```
