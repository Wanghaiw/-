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
run()：strat()调用run方法，如果实例进程时未制定传入target，这时start执行默认的run()方法。
terminate()：不管任务是否完成，立即停止工作进程。

属性：
authkey
daemon：和线程的setDeamon功能一样（将父进程设置为守护进程，当父进程结束时，子进程也结束）。
exitcode(进程在运行时为None、如果为–N，表示被信号N结束）。
name：进程名字。
pid：进程号。
```

### 当前进程与线程
```
import multiprocessing

print(multiprocessing.current_process())

def func():
    print(multiprocessing.current_process())

if __name__ == '__main__':
    p = multiprocessing.Process(target=func)
    p.start()
    
### current_process() 获取当前的进程对象

```

```
# 等待结束 join
# import time
# import multiprocessing
# import threading
#
#
# print('main Process start {}'.format(time.asctime(time.localtime(time.time()))))
#
# def func():
#     print('func start {}'.format(time.asctime(time.localtime(time.time()))))
#     time.sleep(5)
#     print('func end {}'.format(time.asctime(time.localtime(time.time()))))
#
#
# if __name__ == '__main__':
#     p = multiprocessing.Process(target=func)
#     p.start()
#     p.join()
#     time.sleep(5)
#     print('main Process end {}'.format(time.asctime(time.localtime(time.time()))))
#     print(threading.active_count())

# from multiprocessing import Process
# import time
#
#
# def work(name):
#     print('task <%s> is runing' % name)
#     time.sleep(3)
#     print('task <%s> is done' % name)
#
#
# if __name__ == '__main__':
#     p1 = Process(target=work, args=('进程1',))
#     p2 = Process(target=work, args=('进程2',))
#     p3 = Process(target=work, args=('进程3',))
#     p1.start()
#     p2.start()
#     p3.start()
#     p1.join()  # 主进程等，等待p1运行结束
#     # p2.join() #主进程等，等待p2运行结束
#     # p3.join() #主进程等，等待p3运行结束
#     print('主进程结束')


    # 有的同学会有疑问:既然join是等待进程结束,那么我像下面这样写,进程不就又变成串行的了吗?
    # 当然不是了,必须明确：p.join()是让谁等？
    # 很明显p.join()是让主线程等待p的结束，卡住的是主线程而绝非进程p，

    # 详细解析如下：
    # 进程只要start就会在开始运行了,所以p1-p4.start()时,系统中已经有四个并发的进程了
    # 而我们p1.join()是在等p1结束,没错p1只要不结束主线程就会一直卡在原地,这也是问题的关键
    # join是让主线程等,而p1-p3仍然是并发执行的,p1.join的时候,其余p2,p3仍然在运行,等#p1.join结束,可能p2,p3早已经结束了,这样p2.join,p3.join直接通过检测，无需等待
    # 所以3个join花费的总时间仍然是耗费时间最长的那个进程运行的时间




# 当前进程与线程

# import multiprocessing
#
#
#
# def func():
#     print(multiprocessing.current_process().name)
#
#
# if __name__ == '__main__':
#     print(multiprocessing.current_process().name)
#     p = multiprocessing.Process(target=func)
#     p.start()


# 终止进程

import multiprocessing
import time

def func():
    time.sleep(50)
    print(multiprocessing.current_process())


if __name__ == '__main__':
    p = multiprocessing.Process(target=func)
    p.start()
    time.sleep(20)
    #p.terminate()

# 标识

# import multiprocessing
# import os
# import time
#
# def func():
#     time.sleep(10)
#
#
# if __name__ == '__main__':
#
#     print('main pid:',os.getpid())
#     p = multiprocessing.Process(target=func)
#     print(p.pid)
#     p.start()
#     print(p.pid)

```

作业:
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





