# 操作容器

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

## 启动容器
启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（stopped）的容器重新启动。
因为 Docker 的容器实在太轻量级了，很多时候都是随时删除和新创建容器。

### 新建并启动

通过`docker run`命令可以创建并运行一个容器。例如:

```
wangxian@wangxian:~$ docker run ubuntu:latest echo 'Welcome to ubuntu!'
Welcome to ubuntu!
```
这个和本地执行`echo 'Welcome to ubuntu!'`基本是一样的。
另外在启动容器的时候也可以添加一些其他的参数，
```
wangxian@wangxian:~$ docker run -it ubuntu
root@3eed37779ffd:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@3eed37779ffd:/# exit
exit
wangxian@wangxian:~$ 
```
这里我们可可以看到在启动容器的时候，我们添加了`-it`这个参数,进入到交互终端。
其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开。




