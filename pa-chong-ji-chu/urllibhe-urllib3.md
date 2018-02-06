# 整体介绍
urllib是一个包含几个模块来处理请求的库。
分别是：
* urllib.request 发送http请求
* urllib.error 处理请求过程中,出现的异常。
* urllib.parse 解析url
* urllib.robotparser 解析robots.txt 文件

## urllib.request
urllib当中使用最多的模块,涉及请求，响应，浏览器模拟，代理，cookie等功能。
1. 快速请求
```
import urllib.request
url = 'https://www.jianshu.com'
print(urllib.request.urlopen(url).read().decode())
```
urlopen返回对象提供一些基本方法：
read 返回文本数据
info 服务器返回的头信息
getcode 状态码
geturl 请求的url

2.模拟浏览器
需要添加headers头信息，urlopen不支持，需要使用Request。
```
import urllib.request

url = 'https://www.jianshu.com'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.96 Safari/537.36'
}
request = urllib.request.Request(url,headers=headers)
response = urllib.request.urlopen(request)
print(response.info())
```
在urllib里面 判断是get请求还是post请求，就是判断是否提交了data参数。

3.Cookie的使用
```
import urllib.request
import http.cookiejar

url = 'https://www.jianshu.com'
# 创建CookieJar对象
cookie_jar = http.cookiejar.CookieJar()
#使用HTTPCookieProcessor创建cookie处理器，并以其为参数构建opener对象
opener=urllib.request.build_opener(urllib.request.HTTPCookieProcessor(cookie_jar))
# 安装opener
urllib.request.install_opener(opener)
data = urllib.request.urlopen(url)
print(cookie_jar)
```
4.设置代理
```
import urllib.request

url = 'http://httpbin.org/ip'
proxy = {'http':'39.134.108.89:8080','https':'39.134.108.89:8080'}
proxies = urllib.request.ProxyHandler(proxy) # 创建代理处理器
opener = urllib.request.build_opener(proxies,urllib.request.HTTPHandler) # 创建特定的opener对象
urllib.request.install_opener(opener) # 安装全局的opener 把urlopen也变成特定的opener
data = urllib.request.urlopen(url)
print(data.read().decode())
```
## urllib.error

