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
> db.test.find({$or:[{'students':55},{'title':'python_MongoDB教学'}]})
```
$
$
$
$
$
$
$
$
