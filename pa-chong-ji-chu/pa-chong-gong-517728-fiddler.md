# fiddler简介
Fiddler是Web调试工具之一，Fiddler可以也可以让你检查所有的HTTP通讯，设置断点，以及Fiddle所有的“进出”的数据（我一般用来抓包）,Fiddler还包含一个简单却功能强大的基于JScript .NET事件脚本子系统，它可以支持众多的HTTP调试任务。

# 工作原理

![](/assets/947566-f51654e6f0018748.jpg)
Fiddler是以代理WEB服务器的形式工作的,浏览器与服务器之间通过建立TCP连接以HTTP协议进行通信，浏览器默认通过自己发送HTTP请求到服务器，它使用代理地址:127.0.0.1, 端口:8888. 当Fiddler开启会自动设置代理， 退出的时候它会自动注销代理，这样就不会影响别的程序。不过如果Fiddler非正常退出，这时候因为Fiddler没有自动注销，会造成网页无法访问。解决的办法是重新启动下Fiddler。

# 基本界面

![](/assets/基本界面.png)

# 常用功能

## HTTPS监听
## 断点设置
## 创建AutoResponder规则
Fiddler 的AutoResponder tab允许你从本地返回文件，而不用将http request 发送到服务器上。
* 1.打开起点阅读的首页,把它的logo图片保存到本地，并做一些修改。
* 2.在fiddler的会话列表找到对应的请求,把这个会话移动到AutoResponser Tab里面。
* 3.选择Enable rules和Unmatched requests passthrough。
