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


