### 仓库

仓库（Repository）就是集中存放镜像的地方。

### Docker Hub

Docker Hub是官方维护的一个公共仓库，其中包含了大部分的镜像，可以直接下载使用。

### 仓库的基本操作

* 1.注册  

在` https://cloud.docker.com `免费注册Docker账号。
* 2.登录

在命令行通过`docker login`命令进行登录。

* 3.拉取镜像

首先通过`docker search image_name`来进行搜索,然后通过`docker pull` 命令下载到本地.
例如:
```


