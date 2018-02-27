# 介绍及安装
Beautiful Soup 是一个HTML/XML的解析器，主要的功能也是如何解析和提取 HTML/XML 数据。
BeautifulSoup 用来解析 HTML 比较简单，API非常人性化，支持CSS选择器、Python标准库中的HTML解析器，也支持 lxml 的 XML解析器。
Beautiful Soup 3 目前已经停止开发，推荐现在的项目使用Beautiful Soup 4。使用 pip 安装即可：`pip install beautifulsoup4`

# 四大对象种类
Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种:
* Tag
* NavigableString
* BeautifulSoup
* Comment
### 1.Tag
Tag 通俗点讲就是 HTML 中的一个个标签。
```
print(soup.p)
# <p class="title" name="dromouse"><b>The Dormouse's story</b></p>

print(type(soup.p))
# <class 'bs4.element.Tag'>
```
我们可以利用 soup 加标签名轻松地获取这些标签的内容，这些对象的类型是bs4.element.Tag。
对于 Tag，它有两个重要的属性，是 name 和 attrs.
```
print(soup.name)
# [document] soup 对象本身比较特殊，它的 name 即为 [document]
print(soup.head.name)
# head 对于其他内部标签，输出的值便为标签本身的名称
print(soup.p.attrs)
# {'class': ['title'], 'name': 'dromouse'}
print(soup.p['class'])
# ['title']
```



## NavigableString



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

# css选择器
这就是另一种与 find_all 方法有异曲同工之妙的查找方法.

* 写 CSS 时，标签名不加任何修饰，类名前加.，id名前加#

* 在这里我们也可以利用类似的方法来筛选元素，用到的方法是 soup.select()，返回类型是 list

## 1.通过标签名查找
```
print(soup.select('title'))
#[<title>The Dormouse's story</title>]

print(soup.select('a'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

print(soup.select('b'))
#[<b>The Dormouse's story</b>]
```
## 2.通过类名查找
```
print(soup.select('.sister'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```
## 3.通过 id 名查找
```
print(soup.select('#link1'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
```

## 4.组合查找

组合查找即和写 class 文件时，标签名与类名、id名进行的组合原理是一样的，例如查找 p 标签中，id 等于 link1的内容，二者需要用空格分开
```
print(soup.select('p #link1'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
直接子标签查找，则使用 > 分隔

print(soup.select("head > title"))
#[<title>The Dormouse's story</title>]
```
## 5.属性查找

查找时还可以加入属性元素，属性需要用中括号括起来，注意属性和标签属于同一节点，所以中间不能加空格，否则会无法匹配到。
```
print(soup.select('a[class="sister"]'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

print(soup.select('a[href="http://example.com/elsie"]'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
同样，属性仍然可以与上述查找方式组合，不在同一节点的空格隔开，同一节点的不加空格

print(soup.select('p a[href="http://example.com/elsie"]'))
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
```
## 6.获取内容

以上的 select 方法返回的结果都是列表形式，可以遍历形式输出，然后用 get_text() 方法来获取它的内容。
```
soup = BeautifulSoup(html, 'lxml')
print(type(soup.select('title')))
print(soup.select('title')[0].get_text())

for title in soup.select('title'):
    print(title.get_text())
```


=========================================================
```
通过tag标签逐层查找:
soup.select("body a")
找到某个tag标签下的直接子标签
soup.select("head > title")
通过CSS的类名查找:
soup.select(".sister")
通过tag的id查找:
soup.select("#link1")
通过是否存在某个属性来查找:
soup.select('a[href]')
通过属性的值来查找:
soup.select('a[href="http://example.com/elsie"]')
```