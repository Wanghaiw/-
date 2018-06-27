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

## 后台运行
更多的时候，需要让 Docker 在后台运行而不是直接把执行命令的结果输出在当前宿主机下。此时，可以通过添加 -d 参数来实现。

# 定制镜像 

除了使用定制好的镜像外，我们也可以通过定制实现符合自己环境的镜像。
在docker里面通过build方法来生成镜像,在生成镜像之前，我们需要一个Dockerfile脚本，脚本中包含的是一条一条的指令,用来的表示镜像的构建。

这里我们以原先的nginx镜像为基础,使用Dockerfile重新定制一下，
* 1.建立一个Dockerfile文件
* 2.在Dockerfile中编写 `FROM nginx` `RUN echo '<h1>Hello DockerFile!</h1>' > /usr/share/nginx/html/index.html`
这个 Dockerfile 很简单，一共就两行。涉及到了两条指令，FROM 和 RUN。

### FROM 指定基础镜像
所谓定制镜像，那一定是以一个镜像为基础，在其上进行定制。
而 FROM 就是指定基础镜像，因此一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。

### RUN 执行指令
RUN 指令是用来执行命令行命令的。由于命令行的强大能力，RUN 指令在定制镜像时是最常用的指令之一。

这样我们就生成了一个新的镜像。这个Dockerfile比较简单 其中涉及到两个指令`FROM`和`RUN`。
在定制镜像的时候，一般是以一些基础镜像为基础,在这之上进行定制。
这里的`FROM nginx` 就是在`nginx`镜像的基础上进行的定制。
而`RUN` 指执行命令 类似于在shell中执行。
里我们使用了 docker build 命令进行镜像构建。其格式为：
`docker build [选项] <上下文路径/URL/->`
需要注意的是这里的上下文路径 不是生成镜像存放的位置，而是在生成镜像发送给docker引擎的路径。

### 构建镜像 

Dockerfile文件完成之后，通过`Docker build`命令进行构建镜像。
```
wangxian@wangxian:~/wangxian/Docker_test/code1$ docker build -t my_nginx:v1 .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM nginx
 ---> cd5239a0906a
Step 2/2 : RUN echo '<h1>Welcome to Docker!!<h1>' > /usr/share/nginx/html/index.html
 ---> Running in 4d51e06eccd5
Removing intermediate container 4d51e06eccd5
 ---> 9c8135c4b40e
Successfully built 9c8135c4b40e
Successfully tagged my_nginx:v1
wangxian@wangxian:~/wangxian/Docker_test/code1$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
my_nginx            v1                  9c8135c4b40e        5 seconds ago       109MB
ubuntu              latest              113a43faa138        2 weeks ago         81.2MB
nginx               latest              cd5239a0906a        2 weeks ago         109MB
```
在这里我们指定了最终镜像的名称 -t my_nginx:v1，构建成功后，我们可以像之前运行nginx那样来运行这个镜像.
```
wangxian@wangxian:~/wangxian/Docker_test/code1$ docker run -d -p 80:80 my_nginx:v1 
f30c9abd13de09e783f8b7db11833f31180becdb1bc5abc2243dd28f178e8592
```
###上下文环境


### Dockerfile常用指令

在定制镜像时,除了上面所讲到的FROM和RUN之外,还有一些其他的常用的指令。

* 1. COPY 复制文件
COPY 指令将从构建上下文目录中<源路径>的文件复制到新的一层的镜像内的<目标路径>位置。
```
FROM nginx 
RUN mkdir app
COPY ./index.html ./app
```

* 2.CMD容器启动命令
cmd给出的是一个容器的默认的可执行体。也就是容器启动以后，默认的执行的命令。重点就是这个“默认”。意味着，如果docker run没有指定任何的执行命令或者dockerfile里面也没有entrypoint，那么，就会使用cmd指定的默认的执行命令执行。

CMD 指令的格式有两种
```
shell 格式：CMD <命令>
exec 格式：CMD ["可执行文件", "参数1", "参数2"...]

第一种
FROM ubuntu
CMD echo 'Hello CMD!!'
wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run e8c9913f85b4
Hello CMD!
运行之后,会执行语句。但是不支持添加参数

第二种
FROM ubuntu
CMD ["/bin/bash", "-c", "echo 'hello cmd!'"]

wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v1 
Hello CMD!!
wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v1 echo hhhh
hhhh
```
从结果可以发现 使用第二种方式 支持在运行容器是 添加参数 但是会覆盖掉CMD本身的命令。

* 3.ENTRYPOINT 入口命令
ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数。不同的是当指定了 ENTRYPOINT 后，CMD 的含义就发生了改变，不再是直接的运行其命令，而是将 CMD 的内容作为参数传给 ENTRYPOINT 指令，换句话说实际执行时，将变为`ENTRYPOINT  CMD`.

```
FROM ubuntu
CMD ["Hello CMD!!"]
ENTRYPOINT ["echo"]

wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v2 
Hello CMD!!
wangxian@wangxian:~/wangxian/Docker_test/code2$ docker run cc:v2 aaaa
aaaa
```















