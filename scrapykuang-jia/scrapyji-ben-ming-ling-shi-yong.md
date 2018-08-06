Scrapy提供了两种类型的命令。一种必须在Scrapy项目中运行(针对项目(Project-specific)的命令)，另外一种则不需要(全局命令)。全局命令在项目中运行时的表现可能会与在非项目中运行有些许差别(因为可能会使用项目的设定)。

可以通过`scrapy -h 和scrapy <command> -h`,查看每个命令的详细信息。
```
1. scrapy startproject <project_name>
创建scrapy项目的命令,会在当前目录创建一个名为project_name的scrapy的项目。
wangxian@wangxian-VirtualBox:~/wangxian/scrapy_demo$ scrapy startproject my_spider
New Scrapy project 'my_spider', using template directory '/usr/local/lib/python3.5/dist-packages/scrapy/templates/project', created in:
    /home/wangxian/wangxian/scrapy_demo/my_spider

You can start your first spider with:
    cd my_spider
    scrapy genspider example example.com

2.scrapy genspider [-t template] <name> <domain>
在当前项目创建spider文件,该方法获使用默认的模板来生成spider。
wangxian@wangxian-VirtualBox:~/wangxian/scrapy_demo/my_spider$ scrapy genspider qsbk qiushibaike.com
 Created spider 'qsbk' using template 'basic' in module:
  my_spider.spiders.qsbk
  
3.scrapy list
列出当前项目中所有可用的spider。
wangxian@wangxian-VirtualBox:~/wangxian/scrapy_demo/my_spider$ scrapy list
qsbk
4.scrapy crawl <spider_name>
启动scrapy项目
wangxian@wangxian-VirtualBox:~/wangxian/scrapy_demo/my_spider$ scrapy crawl qsbk

```