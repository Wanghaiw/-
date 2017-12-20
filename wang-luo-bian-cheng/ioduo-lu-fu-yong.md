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



