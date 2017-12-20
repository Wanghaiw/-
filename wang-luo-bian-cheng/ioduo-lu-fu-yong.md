# select

## 1.select原理
在多路复用的模型中，常见的有select模型个epoll模型。这两个都是系统接口,由操作系统提供。当然,python的select模块进行了高级封装。
网络通信Unix系统抽象为文件的读写,通常是一个设备，有设备驱动程序提供，驱动可以指定自身的数据是否可用。支持阻塞操作的设备驱动通常会实现一组自身的等待队列，如读/写等待队列用于支持上层所需的block或non-block操作。设备的文件资源如果可用（可读或者可写）则会通知进程，反之则会让进程睡眠，等到数据到来，再唤醒进程。
这些设备的文件描述符被放在一个数组中，然后select调用的时候遍历这个数组，如果有文件描述符可读或者可写就会返回这个文件描述符。当遍历结束之后，如果仍然没有可读或者可写的文件描述符，select会让用户进程睡眠，直到资源可用的时候在唤醒。
select通过一个select()系统调用来监视多个文件描述符的数组，当select()返回后，该数组中就绪的文件描述符便会被内核修改标志位，使得进程可以获得这些文件描述符从而进行后续的读写操作。

## 2.select优缺点
select目前几乎在所有的平台上支持，其良好跨平台支持也是它的一个优点，事实上从现在看来，这也是它所剩不多的优点之一。
select的一个缺点在于单个进程能够监视的文件描述符的数量存在最大限制，在32位电脑为1024，64位是2048.
另外，select()所维护的存储大量文件描述符的数据结构，随着文件描述符数量的增大，其复制的开销也线性增长。
对socket进行扫描是依次扫描，即采用轮询的方法,效率较低。

## 3. 文件描述符
　在网络中，一个socket对象就是1个文件描述符，在文件中，1个文件句柄（即file对象）就是1个文件描述符。其实可以理解为就是一个“指针”或“句柄”，指向1个socket或file对象，当file或socket发生改变时，这个对象对应的文件描述符，也会发生相应改变。


## 示例
select 服务端
```
import select
import socket


sock = socket.socket()
print('socket 创建成功')
sock.bind(('127.0.0.1',8888))
sock.listen(5)

inputs = [sock,]

while True:
    print('等待select 监听')
    readable,writeable,exceptional = select.select(inputs,[],[])
    for s in readable:
        if s is sock:
            conn,addr = sock.accept()
            print('新的连接建立成功')
            inputs.append(conn)
        else:
            data = s.recv(1024)
            print('client_message:',data)
            if data:
                s.send(data)
            else:
                inputs.remove(s)
                s.close()
sock.close()
```
客户端
```
import socket
import time


print("Connect to the server")

server_address = ("127.0.0.1",8888)

#Create a TCP/IP sock

socks = []

for i in range(10):
    socks.append(socket.socket(socket.AF_INET,socket.SOCK_STREAM))

for s in socks:
    s.connect(server_address)

counter = 0

for s in socks:
    counter+=1
    print("client_send:",counter)
    s.send(str(counter).encode())

time.sleep(2)

for s in socks:
    data = s.recv(1024)
    print("server_message:",data)
    if not data:
        print("closing socket ",s.getpeername())
        s.close()
```

# TCP沾包

https://www.cnblogs.com/guobaoyuan/p/6809447.html

# epoll
1. epoll的优点：
没有最大并发连接的限制，能打开的FD(指的是文件描述符，通俗的理解就是套接字对应的数字编号)的上限远大于1024
效率提升，不是轮询的方式，不会随着FD数目的增加效率下降。只有活跃可用的FD才会调用callback函数；即epoll最大的优点就在于它只管你“活跃”的连接，而跟连接总数无关，因此在实际的网络环境中，epoll的效率就会远远高于select和poll。

2. epoll使用参考代码

```
import socket
import select

# 创建套接字
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

# 设置可以重复使用绑定的信息
s.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)

# 绑定本机信息
s.bind(("",7788))

# 变为被动
s.listen(10)

# 创建一个epoll对象
epoll=select.epoll()

# 测试，用来打印套接字对应的文件描述符
# print s.fileno()
# print select.EPOLLIN|select.EPOLLET

# 注册事件到epoll中
# epoll.register(fd[, eventmask])
# 注意，如果fd已经注册过，则会发生异常
# 将创建的套接字添加到epoll的事件监听中
epoll.register(s.fileno(),select.EPOLLIN|select.EPOLLET)


connections = {}
addresses = {}

# 循环等待客户端的到来或者对方发送数据
while True:

    # epoll 进行 fd 扫描的地方 -- 未指定超时时间则为阻塞等待
    epoll_list=epoll.poll()

    # 对事件进行判断
    for fd,events in epoll_list:

        # print fd
        # print events

        # 如果是socket创建的套接字被激活
        if fd == s.fileno():
            conn,addr=s.accept()

            print('有新的客户端到来%s'%str(addr))

            # 将 conn 和 addr 信息分别保存起来
            connections[conn.fileno()] = conn
            addresses[conn.fileno()] = addr

            # 向 epoll 中注册 连接 socket 的 可读 事件
            epoll.register(conn.fileno(), select.EPOLLIN | select.EPOLLET)


        elif events == select.EPOLLIN:
            # 从激活 fd 上接收
            recvData = connections[fd].recv(1024)

            if len(recvData)>0:
                print('recv:%s'%recvData)
            else:
                # 从 epoll 中移除该 连接 fd
                epoll.unregister(fd)

                # server 侧主动关闭该 连接 fd
                connections[fd].close()

                print("%s---offline---"%str(addresses[fd]))
               
 ```
                
2. 说明

EPOLLIN （可读）
EPOLLOUT （可写）
EPOLLET （ET模式）
epoll对文件描述符的操作有两种模式：LT（level trigger）和ET（edge trigger）。LT模式是默认模式，LT模式与ET模式的区别如下：

LT模式：当epoll检测到描述符事件发生并将此事件通知应用程序，应用程序可以不立即处理该事件。下次调用epoll时，会再次响应应用程序并通知此事件。

ET模式：当epoll检测到描述符事件发生并将此事件通知应用程序，应用程序必须立即处理该事件。如果不处理，下次调用epoll时，不会再次响应应用程序并通知此事件。



