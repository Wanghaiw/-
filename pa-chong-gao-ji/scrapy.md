## redis基础知识

### Redis的数据类型
* 字符串
* 散列/哈希
* 列表
* 集合
* 可排序集合


#### 字符串命令
```
set mykey ''cnblogs''   创建变量

get mykey   查看变量

getrange mykey start end   获取字符串，如:get name 2 5 #获取name2~5的字符串

strlen mykey   获取长度

incr/decr mykey    加一减一，类型是int

append mykey ''com''   添加字符串，添加到末尾
```
#### 哈希命令
```
hset myhash name "cnblogs"   创建变量，myhash类似于变量名，name类似于key，"cnblogs"类似于values

hgetall myhash   得到key和values两者

hget myhash  name  得到values

hexists myhash name  检查是否存在这个key

hdel myhash name   删除这个key

hkeys myhash   查看key

hvals muhash   查看values
```

#### 列表


