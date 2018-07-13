Docker常用命令

build：从一个 Dockerfile 创建一个镜像；
commit：从一个容器的修改中创建一个新的镜像；
cp：在容器和本地宿主系统之间复制文件中；
exec：在运行的容器内执行命令；
export：导出容器内容为一个 tar 包；
history：显示一个镜像的历史信息；
images：列出存在的镜像；
import：导入一个文件（典型为 tar 包）路径或目录来创建一个本地镜像；
info：显示一些相关的系统信息；
inspect：显示一个容器的具体配置信息；
kill：关闭一个运行中的容器 (包括进程和所有相关资源)；
load：从一个 tar 包中加载一个镜像；
login：注册或登录到一个 Docker 的仓库服务器；
logout：从 Docker 的仓库服务器登出；
logs：获取容器的 log 信息；
network：管理 Docker 的网络，包括查看、创建、删除、挂载、卸载等；
ps：列出主机上的容器；
pull：从一个Docker的仓库服务器下拉一个镜像或仓库；
push：将一个镜像或者仓库推送到一个 Docker 的注册服务器；
rename：重命名一个容器；
restart：重启一个运行中的容器；
rm：删除给定的若干个容器；
rmi：删除给定的若干个镜像；
run：创建一个新容器，并在其中运行给定命令；
save：保存一个镜像为 tar 包文件；
search：在 Docker index 中搜索一个镜像；
service：管理 Docker 所启动的应用服务，包括创建、更新、删除等；
start：启动一个容器；
stats：输出（一个或多个）容器的资源使用统计信息；
stop：终止一个运行中的容器；
tag：为一个镜像打标签；
top：查看一个容器中的正在运行的进程信息；
unpause：将一个容器内所有的进程从暂停状态中恢复；
update：更新指定的若干容器的配置信息；
version：输出 Docker 的版本信息；
volume：管理 Docker volume，包括查看、创建、删除等；
wait：阻塞直到一个容器终止，然后输出它的退出符。


Docker 把应用程序和依赖打包成一个文件  在运行文件的时候生成一个虚拟容器 

Docker 安装
sudo apt-get install docker.io 

添加当前用户到Docker用户组 
sudo usermod docker $USER 
添加镜像仓库  
etc/docker/daemon.json  
镜像和容器  
获取镜像  
docker pull  
启动容器 
docker run -i -t -d -p 
查看容器的输出信息 
docker logs container id 

定制镜像 
Dockerfile文件  

FROM  指定基础镜像 
RUN   执行的命令 
COPY  复制内容 
CMD   运行容器默认执行的命令 
ENIRYPOINT  指定容器启动的参数 
WORKDIR      指定一个工作目录
ENIRYPOINT + CMD 

仓库 
Docker Hub 官方仓库 
上传镜像 需要登录 
私有仓库  
安装 docker-registry 
启动registry
docker run -d -p 8888:5000 --restart=always --name registry registry
修改tag标签
docker tag hello-world:latest 127.0.0.1:8888/hello-world
上传镜像
docker push 127.0.0.0:8888/hello-world  

下载私有仓库镜像
docker pull 127.0.0.0:8888/hello-world 

registry启动不成功   需要64位的环境  

