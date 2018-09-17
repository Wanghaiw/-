## 学习使用 requests库

**requests 唯一的一个非转基因的 Python HTTP 库，人类可以安全享用。**
**警告：非专业使用其他 HTTP 库会导致危险的副作用，**
**包括：安全缺陷症、冗余代码症、重新发明轮子症、啃文档症、抑郁、头疼、甚至死亡。**

**requests 官方文档 `http://docs.python-requests.org` **

#### GET 请求
```
import requests
response = request.get('https://www.baidu.com')
print(response.status_code)
print(response.text)
```

#### POST 请求
```
import requests
data = {'key1':'value1'}
response = request.post('https://www.baidu.com')
print(response.status_code)
print(response.text)
```



