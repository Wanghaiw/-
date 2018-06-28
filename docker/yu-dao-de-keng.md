# docker-compose up报错 版本不匹配

sudo rm /usr/bin/docker-compose

curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose

chmod +x /usr/bin/docker-compose


