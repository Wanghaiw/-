# 总结常用的操作符

## 1.比较操作符
$eq 匹配等于（=）指定值的文档
```
> db.test.find({'students':{$eq:55}})
```
返回students等于55 的文档。
$gt 匹配大于(>)指定值的文档
$gte 匹配大于等于指定值的文档
$lt 匹配小于指定值的文档
$lte 小于等于
$ne 不等于
$in 匹配数组中的任意值
```
> db.test.find({'students':{$in:[55,54,56]}})
```
$nin 不匹配数组中的值

## 2.逻辑操作符

$or 或条件查询
```
查询students大于等于55或者title等于python_mongodb教学的文档
> db.test.find({$or:[{'students':{$gte:55}},{'title':'python_MongoDB教学'}]})
```
$and 与条件查询
$not 查询和表达式不匹配的文档
$nor 查询与任一表达式都不匹配的文档
```
> db.test.find({$nor:[{'students':{$gte:57}},{'title':'python_MongoDB教学'}]})
```
## 更新操作符
$inc 将文档中的某个field对应的value自增/减某个数字amount
```
> db.test.update({'class':'爬虫2班'},{$inc:{'students':1}}))
```
$mul 将文档中的某个field对应的value做乘法操作，同上。
$rename 重名名文档中指定的字段名字
$set 更新文档中的某一个字段，而不是全部替换
```
> db.test.update({'students':116},{$set:{'students':56}})
```
