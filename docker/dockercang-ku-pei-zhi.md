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
wangxian@wangxian-VirtualBox:~$ docker search nginx
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        8834                [OK]                
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1348                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   579                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   380                                     [OK]
```
可以看到返回了很多包含关键字的镜像，其中包括镜像名字、描述、收藏数（表示该镜像的受关注程度）、是否官方创建、是否自动创建。
然后通过`docker pull nginx` 进行下载。

* 4.推送镜像
在登录用户之后,可以通过`docker push`命令,将自己的镜像推送到Docker Hub。

`docker tag hello_docker:latest wangxianii/hello_docker:1.0`

这里需要注意的是在进行镜像提交之前需要进行设置用户,然后再进行提交。
`docker push wangxianii/hello_docker:1.0` 
提交完成之后,可以在网页当中打开仓库进行查看。
另外`wangxianii` 换成自己用户名。


### 私有仓库 
刚刚我们上面所讲到的是基于Docker Hub的公共仓库,对于不想公开的镜像我们可以创建本地仓库使用。




