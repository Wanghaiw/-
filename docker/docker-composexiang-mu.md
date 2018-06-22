## 介绍

## 安装

Compose 支持 Linux、macOS、Windows 10 三大平台。
Compose 可以通过 Python 的包管理工具 pip 进行安装，也可以直接下载编译好的二进制文件使用，甚至能够直接在 Docker 容器中运行。

执行 `pip install -U docker-compose` 命令后,用`docker-compose --version`查看版本信息。

输出 `docker-compose version 1.20.0, build ca8d3c6`。


## 使用 
接下来我们通过docker-compose来实现一个简单的web服务。
在开始工作之前，我们需要编辑好必要的3个文件。
第一步，因为应用将要运行在一个满足所有环境依赖的 Docker 容器里面，那么我们可以通过编辑 `Dockerfile` 文件来指定 Docker 容器要安装内容。内容如下：
```
FROM python:3.6-alpine
ADD . /code
WORKDIR /code
RUN pip install redis flask
CMD ["python", "app.py"]
```
首先导入python的基础镜像,然后 指定工作目录,在运行pip命令 安装环境。
第二步通过`docker-compose.yml`文件吧所有的东西关联起来，它描述了应用的构成（一个 web 服务和一个数据库）、使用的 Docker 镜像。
```
version: '3'
services:

  web:
    build: .
    ports:
     - "5000:5000"

  redis:
    image: "redis:alpine"
```

第三步编写`app.py`文件 
```
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = redis.incr('hits')
    return 'Hello World! 该页面已被访问 {} 次。\n'.format(count)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```


