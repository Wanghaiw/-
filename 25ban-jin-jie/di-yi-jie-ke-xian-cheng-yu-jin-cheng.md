作业




# epoll

## 什么是epoll
epoll是Linux内核为处理大批量文件描述符而作了改进的poll，是Linux下多路复用IO接口select/poll的增强版本，它能显著提高程序在大量并发连接中只有少量活跃的情况下的系统CPU利用率。

epoll是一种I/O事件通知机制

I/O事件 
基于file descriptor，支持file, socket, pipe等各种I/O方式
当文件描述符关联的内核读缓冲区可读，则触发可读事件，什么是可读呢？就是内核缓冲区非空，有数据可以读取
当文件描述符关联的内核写缓冲区可写，则触发可写事件，什么是可写呢？就是内核缓冲区不满，有空闲空间可以写入

通知机制 
通知机制，就是当事件发生的时候，去通知他
通知机制的反面，就是轮询机制

以上两点结合起来理解
epoll是一种当文件描述符的内核缓冲区非空的时候，发出可读信号进行通知，当写缓冲区不满的时候，发出可写信号通知的机制

# 为什么需要并发
`http://mingyangshang.github.io/2016/01/09/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B/`


# 进程

## 定义
进程，是执行中的计算机程序。也就是说，每个代码在执行的时候，首先本身即是一个进程。


## python模拟耗时任务

```
import time

print('main_task start:',time.asctime(time.localtime(time.time())))


def func():
    print('sub_task start:',time.asctime(time.localtime(time.time())))
    time.sleep(5)
    print('sub_task end:',time.asctime(time.localtime(time.time())))


func()
time.sleep(5)
print('main_task end:',time.asctime(time.localtime(time.time())))

## asctime() 函数接受时间元组并返回一个可读的形式为"Tue Dec 11 18:07:14 2008" 的字符串
## localtime 格式化时间戳为本地的时间
## time      返回当前的时间戳

```

## 使用进程执行耗时任务

```
import time
from multiprocessing import Process


print('main_task start:',time.asctime(time.localtime(time.time())))

def func():
    print('sub_task start:',time.asctime(time.localtime(time.time())))
    time.sleep(5)
    print('sub_task end:',time.asctime(time.localtime(time.time())))


if __name__ == '__main__':

    pp = Process(target=func)
    pp.start()
    time.sleep(5)
    print('main_task end:',time.asctime(time.localtime(time.time())))
    
## Process 进程对象  
## 创建子进程时，只需要传入一个执行函数和函数的参数，创建一个Process实例，用start()方法启动。
## 
```




