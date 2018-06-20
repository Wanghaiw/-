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


