# 插入数据
在MongoDB中，可以使用insert()方法和save方法插入一个文档到MongoDB集合中，如果此集合不存在，MongoDB会自动为你创建。
先用命令行连接到MongoDB,再进入`tz_mongo`数据库。
`use tz_mongo`
然后插入一个文档到test集合，如果test集合不存在，这个操作会自己创建test集合。
例如:
```
> db.test.insert({'class':'爬虫1班','students':50})
> db.test.save({'class':'爬虫1班','students':50})
> db.test.find()
{ "_id" : ObjectId("5a586e8522dbc47846dd8e1f"), "class" : "爬虫1班", "students" : 50 }
{ "_id" : ObjectId("5a586eae22dbc47846dd8e20"), "class" : "爬虫1班", "students" : 50 }
```
如果插入的文档不包含_id字段，mongo命令行会自动加上这个字段到文档中，并且这个字段的值是根据ObjectId生成。

