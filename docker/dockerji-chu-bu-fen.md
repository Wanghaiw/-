### 环境配置的问题

在软件开发的过程当中,经常会遇到的一个问题，就是环境配置的问题，例如一个python程序，首先要有python引擎，以及其他各种依赖、和环境变量。如果不能保证环境的一致性，那可能就会出现，「这段代码在我机器上没问题啊」。
所以要想办法从根本上解决这个问题，也就是在安装的时候，把原始环境也获取到。

### 虚拟机

虚拟机就是解决环境问题的一种方法，它可以一个操作系统当中运行另外一个操作系统。例如在windows里面运行linux环境。

而这种解决方法有几个缺点：

* 1.资源占用比较大 虚拟机需要占用一部分内存和磁盘空间，当它运行的时候，其他程序是不能使用这些资源的。
* 2.步骤比较多，因为虚拟机启动的是一个完整的操作系统,所以一些系统级别的操作无法跳过。

### linux容器
因为虚拟机的一些缺点，所以在linux当中又有了一种虚拟化技术:容器。
容器不是一个完整的操作系统，而是对进程的隔离。对于容器里面的进程来讲，它所接触到的资源全部都是虚拟的。

由于容器是进程级别的，所以相对于虚拟机来说有很多优点。

* 1.资源占用少 
* 2.体积小
* 3.启动快  

### 什么是Docker
Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。
Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。

### Docker基本配置

### 1.安装

在ubuntu下面执行 `wget -qO- https://get.docker.com/ | sh` 命令安装Docker。
如果命令的方式无法安装,也可以使用`apt-get install docker.io` 进行安装。


安装完成后使用 `sudo docker run hello-world` 来测试是否安装成功。

### 添加用户组
默认情况下，docker 命令会使用 Unix socket 与 Docker 引擎通讯。而只有 root 用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 root 用户。因此，更好地做法是将需要使用 docker 的用户加入 docker 用户组。

`sudo usermod -aG docker $USER`
执行之后 重启服务
注意：这里不用更改 $USER 这个参数，$USER 这个环境变量就是指当前用户名

### 添加镜像加速
在国内拉取Docker Hub镜像时,速度会比较慢，此时一般需要配置镜像加速器。
配置阿里云Docker镜像加速器
打开 开发者平台(https://dev.aliyun.com/search.html) – 管理中心 – 加速器 。可以看到"您的专属加速器地址"`https://xxxxxxx.mirror.aliyuncs.com`
然后打开`/etc/docker/daemon.json`文件(没有时新建该文件)
在文件中写入
```/
{
  "registry-mirrors": ["https://xxxxxxx.mirror.aliyuncs.com"]
}
```
最后重启服务。


### 2.使用镜像
#### 2.1.镜像
我们都知道，操作系统分为内核和用户空间。对于 Linux 而言，内核启动后，会挂载 root 文件系统为其提供用户空间支持。而 Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu 16.04 最小系统的 root 文件系统。

#### 2.2 获取镜像及运行

Docker Hub 上有大量的高质量的镜像可以用，这里我们就说一下怎么获取这些镜像并运行。
`docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]`

* Docker 镜像仓库地址：地址的格式一般是 <域名/IP>[:端口号]。默认地址是 Docker Hub。
* 仓库名：如之前所说，这里的仓库名是两段式名称，即 <用户名>/<软件名>。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。

例如：`docker pull ubuntu:16.04`
就是获取官方仓库当中library/ubuntu 仓库中标签为 16.04 的镜像。

当获取完镜像之后可以通过`docker run`命令来运行。

例如：`docker run ubuntu:16.04 cat /etc/os-release`
在运行之后会输出ubuntu的系统版本信息。

#### 2.4 删除镜像
对于一些用不到的镜像,可以通过`docker image rm`命令进行删除。
`docker image rm [选项] <镜像1><镜像2>`
其中<镜像>可以是镜像的短ID,也可以是镜像名字.
在删除之前通过`docker image ls`获取本地环境的所有镜像.
```
wangxian@wangxian:~$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              16.04               5e8b97a2a082        2 weeks ago         114MB
wangxian@wangxian:~$ docker image rm 5e8b97a2a082      
Untagged: ubuntu:16.04
Untagged: ubuntu@sha256:b050c1822d37a4463c01ceda24d0fc4c679b0dd3c43e742730e2884d3c582e3a
Deleted: sha256:5e8b97a2a0820b10338bd91674249a94679e4568fd1183ea46acff63b9883e9c
Deleted: sha256:ef572e1ba2ecca900f0ec3db00e997de12dd380ce3e360b5813fd75920232359
Deleted: sha256:98fc4d5421178c7be7d5718d2d44abba8053dc5c712e51658fe5b872675b4f7a
Deleted: sha256:7b2cc05dfd889e28234f8831c80ac20cf299d5bbebbbac013f8f7d2b7abc0d65
Deleted: sha256:6b0187d1cdff63eb5966ac72bf4ccd96150586c1409eb858bb98783f02018ee7
Deleted: sha256:644879075e24394efef8a7dddefbc133aad42002df6223cacf98bd1e3d5ddde2
```

另外也可以配合 `dokcer image ls -q`来使用.
`docker image rm $(docker image ls -q)`
删除所有的镜像



### 3.操作容器

* 2.容器

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。




`docker image ls` 列出当前已经下载下来的镜像，列表包含了 仓库名、标签、镜像 ID、创建时间 以及 所占用的空间。

#### 启动容器（后台模式）

`docker run -d -p 80:80 nginx`  在后台运行nginx,把容器的80端口映射到宿主机 
运行之后可以通过`docker ps`命令 查看正在运行的容器
也可以通过 `docker logs 容器id` 查看容器的标准输出

### 删除容器
在删掉一个容器之前需要保证当前容器没有在运行,如果在运行的话需要在删除之前,通过`docker stop 容器id` 来停止运行,然后通过`docker rm 容器id` 来删除容器.

`docker rm $(docker ps -a -q)` 删除所有的容器

### 删除镜像
和删除容器基本类似,在删除镜像之前要保证没有容器在使用当前镜像,然后在通过`docker rmi image id` 来删除镜像。

`docker rmi $(docker images -q)` 删除所有镜像














