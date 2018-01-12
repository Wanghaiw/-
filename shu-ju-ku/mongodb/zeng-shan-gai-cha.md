# 插入数据
在MongoDB中，可以使用insert()方法和save方法插入一个文档到MongoDB集合中，如果此集合不存在，MongoDB会自动为你创建。
先用命令行连接到MongoDB,再进入`tz_mongo`数据库。
`use tz_mongo`
然后插入一个文档到test集合，如果test集合不存在，这个操作会自己创建test集合。
例如:
```
db.test.insert({'class':'爬虫1班','students':50})
db.test.save({'class':'爬虫1班','students':50})
```
