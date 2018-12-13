##  Android模拟器的安装和介绍

夜神安卓模拟器（夜神模拟器），是全新一代的安卓模拟器，与传统安卓模拟器相比，基于android4.4.2,兼容X86/AMD,在性能、稳定性、兼容性等方面有着巨大优势

下载页面地址   ` https://www.yeshen.com/`

## 抓包工具

#### fiddler的安装和介绍
下载地址:`https://www.telerik.com/download/fiddler`

介绍
Fiddler是强大的抓包工具，它的原理是以web代理服务器的形式进行工作的，使用的代理地址是：127.0.0.1，端口默认为8888，我们也可以通过设置进行修改。

https证书安装
默认情况Fiddler只能抓取HTTP请求，需要安装证书之后才能抓取HTTPS请求。
打开Fiddler, Tools-> Fiddler Options 
选中"Decrpt HTTPS traffic",    Fiddler就可以截获HTTPS请求
选中"Allow remote computers to connect".  是允许别的机器把HTTP/HTTPS请求发送到Fiddler上来
配置完后记得要重启Fiddler。

模拟器抓包
首先在安装fiddler的机器上面获取IP地址。如在windows系统通过ipconfig获取ip地址。
然后在模拟器的网络wifi设置中打开代理设置。
另外把第一步获取的ip地址写入代理服务器主机名，fiddler开启的端口写入代理服务器端口。
最后使用模拟器的浏览器打开:`http://192.168.1.96:8888` 点"FiddlerRoot certificate" 然后安装证书。

#### mitmproxy抓包工具的安装和介绍

#### 安装
```
安装环境：
1. 基于python环境
2.在windows操作系统需要安装Microsoft Visual C++ V14.0 以上
3.linux操作系统可以直接基于python安装
```

#### 介绍
```
1. 和正常的代理一样转发请求,保障服务端和客户端的通信。
2. 拦截请求，修改请求,拦截返回,修改返回
3. 可以载入自定义python脚本
```




## Appium 移动端自动化测试工具安装及介绍

#### 介绍
```
1、appium 是一个自动化测试开源工具，支持 iOS 平台和 Android 平台上的原生应用，web应用和混合应用。
2、appium是一个跨平台的工具：它允许测试人员在不同的平台（iOS，Android）使用同一套API来写自动化测试脚本，这样大大增加了iOS和Android测试套件间代码的复用性。
```

#### 安装
下载页面地址： `http://appium.io/`
选择对应的版本进行下载安装


## Android 开发工具环境安装

SDK：（software development kit）软件开发工具包。是软件开发工程师用于为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件的开发工具的集合。

Android SDK 指的是Android专属的软件开发工具包。

#### 配置jdk环境

下载地址：`https://www.oracle.com/technetwork/cn/java/javase/downloads/jdk8-downloads-2133151-zhs.html`










