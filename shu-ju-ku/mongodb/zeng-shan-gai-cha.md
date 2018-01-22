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
在插入的文档中，如果不指定_id参数，那么 MongoDB 会为此文档分配一个唯一的ObjectId。
_id为集合中的每个文档唯一的12个字节的十六进制数。

# 修改数据
MongoDB 使用 update() 和 save() 方法来更新集合中的文档。接下来让我们详细来看下两个函数的应用及其区别。
update() 方法。

## update方法
update方法用于更新已经存在的文档。语法如下：




