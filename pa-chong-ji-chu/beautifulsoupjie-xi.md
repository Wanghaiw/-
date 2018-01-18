# 搜索文档
Beautiful Soup定义了很多搜索方法,这里着重介绍2个: find() 和 find_all() .其它方法的参数和用法类似。
```
html_doc = """
<html><head><title>The Dormouse's story</title></head>

<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc)
```
使用find_all等类似的方法可以查找想要的文档内容。
在介绍find_all方法之前，先介绍一下过滤器的类型。
## 字符串
最简单的过滤器是字符串。在搜索方法中传入一个字符串参数,BeautifulSoup会查找与字符串完整匹配的内容。
例如:
```
查找所有的b标签。
soup.find_all('b')
# [<b>The Dormouse's story</b>]
```
## 正则表达式
find_all方法可以接受正则表示式作为参数，BeautifulSoup会通过match方法来匹配内容。
```
匹配以b开头的标签
for tag in soup.find_all(re.compile('^b')):
    print(tag.name)
#  body  b
匹配包含t的标签
for tag in soup.find_all(re.compile('t')):
    print(tag.name)
# html  title
```
## 列表
find_all方法也能接受列表参数，BeautifulSoup会将与列表中任一元素匹配的内容返回。
```
查找a标签和b标签
for tag in soup.find_all(['a','b']):
    print(tag.name)
# b a a a
```
## 方法
如果没有合适的过滤器,我们也可以自己定义一个方法，方法只接受一个元素参数。
```
匹配包含class属性，但是不包括id属性的标签。
def has_class_but_no_id(tag):
    return tag.has_attr('class') and not tag.has_attr('id')

print([tag.name for tag in soup.find_all(has_class_but_no_id)])
# ['p','p','p']
```

