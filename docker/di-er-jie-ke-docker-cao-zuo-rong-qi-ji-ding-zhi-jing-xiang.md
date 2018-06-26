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
在交互模式下，用户可以通过所创建的终端来输入命令。


### 启动终止的容器

除了启动新容器之外,也可以通过`docker stop container—id`启动之前运行结束的容器。
所以在启动之前,需要用`docker ps -a` 获取所有运行过的容器信息。
```
wangxian@wangxian:~$ docker ps -a
76c99571cfcd        nginx               "nginx -g 'daemon of…"   35 minutes ago      Exited (0) 35 minutes ago                          my_nginx
wangxian@wangxian:~$ docker container start 76c99571cfcd
76c99571cfcd
wangxian@wangxian:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
76c99571cfcd        nginx               "nginx -g 'daemon of…"   37 minutes ago      Up 40 seconds       0.0.0.0:8080->80/tcp   my_nginx
```
注意：`docker ps` 是获取正在运行的容器,加-a参数表示是所有的容器。

## 删除容器
使用`docker rm container—id`删除一个终止状态的容器。
需要注意的是如果删除的容器正在运行 需要在删除的时候加上`-f`参数。
```
wangxian@wangxian:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
76c99571cfcd        nginx               "nginx -g 'daemon of…"   5 hours ago         Up 5 hours          0.0.0.0:8080->80/tcp   my_nginx
wangxian@wangxian:~$ docker rm 76c99571cfcd
Error response from daemon: You cannot remove a running container 76c99571cfcd1b59a69851e889a10d2f794d59688bc102cd4b06b8c0559cf4a4. Stop the container before attempting removal or force remove
wangxian@wangxian:~$ docker rm 76c99571cfcd -f
76c99571cfcd
wangxian@wangxian:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
另外也可以使用` docker container prune`命令删除所有处于终止状态的容器。



