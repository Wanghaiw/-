# docker-compose up报错 版本不匹配

sudo rm /usr/bin/docker-compose

curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose

chmod +x /usr/bin/docker-compose

# 配置Docker Machine 

默认是不支持,在Virtualbox里面创建Docker主机.需要使用VM 并开启虚拟化设置。

开启之后在执行`docker-machine create  virtualbox test` 命令时会报错.
`No default Boot2Docker ISO found locally`

这是需要手动下载boot2docker文件,地址`https://github.com/boot2docker/boot2docker/releases`



