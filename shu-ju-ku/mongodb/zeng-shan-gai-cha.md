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
update方法用于更新已经存在的文档。
```
我们先在test里面插入条数据，然后再进行修改。
> db.test.insert({'title':'MongoDB教学'})
WriteResult({ "nInserted" : 1 })
> db.test.update({'title':'MongoDB教学'},{$set:{'title':'tz_MongoDB教学'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
然后再查看数据发现数据已经被修改。
这里set的作用是指更新文档当中的某一个字段，如果不使用set，文档会被直接替换。
update方法默认只更新一个文档.如果需要更新多个文档，需要指定multi参数

## save方法
save() 方法通过传入的文档来替换已有文档。
```
> db.test.save({'_id':ObjectId("5a586e8522dbc47846dd8e1f"),'class':'爬虫1班','students':56})
```
# 查询数据
mongo查询数据使用的主要方法就是find。find方法以分结构化的方式来显示所有文档。
find方法可以传入查询条件来进行数据查询。也可以不传。
```
> db.test.find()
默认返回20条数据
> db.test.findOne()
返回一条数据
> db.test.find().limit(num)
返回num条数据，不超过20条
> db.test.find({'class':'爬虫1班'})
通过给定的字段查询
> db.test.find({'students':{$gte:58}})
查询students大于等于58的数据
> db.test.find({$or:[{'students':58},{'students':55}]})
or操作符
```




