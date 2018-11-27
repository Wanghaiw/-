# 介绍

这个框架是一个国人编写的强大的网络爬虫系统并带有强大的WebUI。
采用Python语言编写，分布式架构，支持多种数据库后端，
强大的WebUI支持脚本编辑器，任务监视器，项目管理器以及结果查看器。
官方文档：http://docs.pyspider.org/en/latest/
中文文档：http://www.pyspider.cn
网络请求使用的是requests包，因此python环境中需要安装有requests

# 安装和运行
通过pip install pyspider命令进行安装。
安装完成之后  在命令行输入pyspider启动环境
启动环境,在浏览器打开localhost:5000 进入pyspider的web页面。

# phantomjs 安装
sudo apt-get install phantomjs  

# 常见问题 


# 创建项目
在web控制台点create按钮新建任务，项目名自定义。本例项目名为tz_spider。
然后点击tz_spider进入到代码编辑页面.
大多数情况下，一个项目就是你针对一个网站写的一个爬虫脚本．
项目有五个状态:TODO,STOP,CHECKING,DEBUG,RUNNING
TODO- 当一个脚本刚刚被创建时的状态
STOP- 你可以设置项目状态为STOP让项目停止运行
CHECKING- 当一个运行中的项目被编辑时项目状态会被自动设置成此状态并停止运行．
DEBUG/RUNNING- 这两状态都会运行爬虫，但是他们之间是有区别的．一般来说调试阶段用DEBUG状态，线上用RUNNING状态．
除此之外
rate-表示每秒执行多少个请求
burst-设置并发数量
如果想删除一个项目的话，只需要把group设置为delete并且吧项目状态设置为STOP，24小时后系统会自动删除该项目。

# 调试页面 
创建项目完成之后,点击项目名字可以进入代码编辑页面。
在调试页面我们可以看到一一些默认的代码。
然后可以对代码进行修改,并尝试运行.
当完成脚本编写，调试无误后，保存当前脚本！然后返回到控制台首页。
直接点击项目状态status那栏，把状态由TODO改成running。
最后点击项目最右边那个RUN按钮启动项目。

# ssl异常
在crawl方法中添加validate_cert =False 参数


## 默认代码结构分析
1.on_star 方法是入口代码。当在web控制台点击run按钮时会执行此方法。
2.crawl 这个方法是调用API生成一个新的爬取任务，这个任务被添加到待抓取队列。
3.index_page 这个方法获取一个Response对象。 response.doc是pyquery对象的一个扩展方法。pyquery是一个类似于jQuery的对象选择器。
4. detail_page 返回一个结果集对象。这个结果默认会被添加到resultdb数据库（如果启动时没有指定数据库默认调用sqlite数据库）。你也可以重写on_result(self,result)方法来指定保存位置
5.@every(minutes=24*60, seconds=0) 这个设置是告诉scheduler（调度器）on_start方法每天执行一次。
6.@config(age=10 * 24 * 60 * 60) 这个设置告诉scheduler（调度器）这个request（请求）过期时间是10天，10天内再遇到这个请求直接忽略。这个参数也可以在self.crawl(url, age=10*24*60*60) 和 crawl_config中设置。

# 使用Phantomjs
对于一些复杂的页面,无论是分析 API 请求的地址，还是渲染时进行了加密，让直接抓取请求非常麻烦。这时候我们可以使用Phantomjs.
使用起来也非常的简单.在调用crawl方法时,添加参数fetch_type=js参数.就可以开启phantomjs.
除了使用浏览页面之外,也可以在页面上执行js脚本.
```
def on_start(self):
    self.crawl('http://movie.douban.com/explore#more',
               fetch_type='js', js_script="""
               function() {
                 setTimeout("$('.more').click()", 1000);
               }""", callback=self.index_page) 

# 数据存储

默认情况下,pyspider会把数据保存到当前路径的sqlit数据库.
当需要对数据存储进行修改时,可以重写on_result 方法,对数据进行自定义保存。
例如:
def on_result(self, result):
    sql = 'insert into v2ex(url, title) values(%s, %s )'
    insert(sql, (result['url'], result['title']))
```











