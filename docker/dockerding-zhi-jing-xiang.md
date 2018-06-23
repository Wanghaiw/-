### 定制镜像

除了使用定制好的镜像外，我们也可以通过定制实现符合自己环境的镜像。
在docker里面通过build方法来生成镜像,在生成镜像之前，我们需要一个Dockerfile脚本，脚本中包含的是一条一条的指令,用来的表示镜像的构建。

这里我们以原先的nginx镜像为基础,使用Dockerfile重新定制一下，
* 1.建立一个Dockerfile文件
* 2.在Dockerfile中编写 `FROM nginx` `RUN echo '<h1>Hello DockerFile!</h1>' > /usr/share/nginx/html/index.html`
* 3.然后在执行`docker build -t wangxian/nginx .`

这样我们就生成了一个新的镜像。这个Dockerfile比较简单 其中涉及到两个指令`FROM`和`RUN`。
在定制镜像的时候，一般是以一些基础镜像为基础,在这之上进行定制。
这里的`FROM nginx` 就是在`nginx`镜像的基础上进行的定制。
而`RUN` 指执行命令 类似于在shell中执行。
里我们使用了 docker build 命令进行镜像构建。其格式为：
`docker build [选项] <上下文路径/URL/->`
需要注意的是这里的上下文路径 不是生成镜像存放的位置，而是在生成镜像发送给docker引擎的路径。











