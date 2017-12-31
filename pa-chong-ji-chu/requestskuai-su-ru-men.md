# 1.requests介绍
requests 唯一的一个非转基因的 Python HTTP 库，人类可以安全享用。

警告：非专业使用其他 HTTP 库会导致危险的副作用，包括：安全缺陷症、冗余代码症、重新发明轮子症、啃文档症、抑郁、头疼、甚至死亡。

# 2.快速上手
## 2.1 发送请求
使用requests发送请求非常简单。
第一步导入requests库
```import requests```
然后,尝试获取某个网页.这里我们来获取`httpbin`的首页
。
```r = requests.get('http://httpbin.org')```
执行完之后,我们有了一个叫r的Response对象。我们可以从这个对象中获取所有我们想要的信息。
requests发送其他的请求也是一样的方便.
例如: post请求和put请求等等。
```
r = requests.post("http://httpbin.org/post")
r = requests.put("http://httpbin.org/put")
```

## 2.2 响应内容
### 2.2.1 文本响应
读取服务器响应的内容.以百度首页为例。
```
>>> import requests
>>> r = requests.get('https://www.baidu.com')
>>> print(r.text)
.......
```
请求发出后，Requests 会基于 HTTP 头部对响应的编码作出有根据的推测。当你访问 r.text 之时，Requests 会使用其推测的文本编码。
观察他的响应内容可以发现中文部分是乱码,使用`r.encoding`方法可以查看他的编码。
```
>>> print(r.encoding)
ISO-8859-1
```
另外我们也可以使用`r.encoding` 属性来改变他.
`r.encoding = 'utf-8'`
这个时候我们在访问`r.text`时,可以发现中文已经能够正常显示.

### 2.2.2 二进制响应内容
除了text方法之外,我们还可以通过content方法,来访问响应体。

```
>>> r.content
```
content方法一般用于请求图片,mp3，mp4等资源。

### 2.2.3 JSON响应内容
requests中有一个内置的JSON解码器,帮助我们处理JSON格式的数据。
```
>>> import requests
>>> r = requests.get('http://httpbin.org/ip')
>>> r.json()
{'origin': '110.53.163.24'}
```
需要注意的是,如果响应内容不是json格式数据,在调用json方法,会抛出异常`ValueError`.

## 2.3 添加请求头
在我们发出一个请求的时候,一般都会有一个Request headers 也就是我们的请求头信息。里面包含一个User-Agent,表示发送请求的工具.如果我们不设置的话。在我们使用requests请求一个网站的时，默认是python的模块版本号。
```
>>> import requests
>>> r = requests.get('http://httpbin.org/ip')
```
通过fiddler(一个抓包工具)可以观察到这个时候我们的请求信息是`User-Agent: python-requests/2.18.1`。所以有一些网站会根据User-Agent来判断是否接受一个请求。
所以我们需要伪装User-Agent信息。
```
>>> import requests
>>> r = requests.get('http://httpbin.org/ip')
>>> headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36'}
```
这个我们再观察会发现,我们请求头信息里面的User-Agent已经是我们所设置的信息。

## 2.4 POST请求
当我们请求一个网站的时候,有一些页面需要我们发送一些数据给它,这就是我们所说的POST请求。
```
>>> import requests
>>> data = {'key1':1,'key2':2}
>>> r = requests.post('http://httpbin.org/post',data=data)
>>> r.text
{
  ...
  "form": {
    "key1": [
      "value1",
      "value2"
    ]
  },
  ...
}
```
和添加请求头一样,我们在请求的时候传给他一个字典就可以了。

## 2.5 响应状态码
在HTTP1.1中,状态码大致分为5类
100-199 用于指定客户端应相应的某些动作。 
200-299 用于表示请求成功。 
300-399 用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 
400-499 用于指出客户端的错误。 
500-599 用于支持服务器错误。 
常见的有:
200 (OK/正常)
301 重定向
400 请求错误
401 未授权
403 禁止访问
404 没有找到
500 服务器内部错误

在requests里面,通过status_code方法可以查看状态码.
```
>>> import requests
>>> r = requests.get('http://httpbin.org/ip')
>>> r.status_code
200
```
### 2.6 响应头信息
和我们前面所讲的请求头信息类似.可以通过headers获取响应头信息。
```
>>> import requests
>>> r = requests.get('http://httpbin.org/ip')
>>> r.headers
{'Connection': 'keep-alive', 'Server': 'meinheld/0.6.1', 'Date': 'Sun, 31 Dec 2017 08:17:49 GMT', 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true', 'X-Powered-By': 'Flask', 'X-Processed-Time': '0.000802040100098', 'Content-Length': '453', 'Via': '1.1 vegur'}
>>> r.headers.get('connection')
'keep-alive'
```
另外HTTP头部是忽略大小的，所以我们在访问的时候也可以不管大小写。

### 2.7 重定向
在requests里面默认会自动处理所用的重定向.








