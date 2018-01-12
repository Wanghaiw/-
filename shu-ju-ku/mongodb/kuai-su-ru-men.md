# MongoDB简介
MongoDB是一个开源的文档类型数据库，它具有高性能，高可用，可自动收缩的特性。MongoDB能够避免传统的ORM映射从而有助于开发。

## 文档
在MongoDB中，一行纪录就是一个文档，它是一个由键值对构成的数据结构，MongoDB文档与JSON对象类似。键的值可以包含其他的文档，数组，文档数组。
```
{
        "_id" : ObjectId("5a55c9857c1ccc05bc71e9b0"),
        "aid" : 2,
        "view" : "--",
        "danmaku" : 3564,
        "reply" : 3564,
        "favorite" : 2763,
        "coin" : 924,
        "share" : 1511,
        "now_rank" : 0,
        "his_rank" : 0,
        "no_reprint" : 0,
        "copyright" : 2
}
```
## 集合
MongoDB在集合中存储文档。集合类似于关系数据库中的表。然而，与表不同的是集合不要求它里面的文档具有相同的结构。
在MongoDB中，存储在集合中的文档必然有一个唯一的_id字段作为主键。

# MongoDB安装
MongoDB2.2版本之前不支持Windows XP，本教程使用的版本是最新的3.4的版本。为了方便操作和理解，所以选择在Windows讲解，生产环境请使用Linux版本。

## 设置MongoDB环境
MongoDB需要一个目录来保存数据，默认的数据目录是\data\db。在我的机器上使用下面的目录作为数据目录。
`D:\data`
你可以在启动MongoDB的时候为它指定一个数据目录。例如我们用如下命令启动MongoDB:
`D:\MongoDB\bin\mongod.exe --dbpath D:\data`
## 连接MongoDB
使用mongo.exe命令连接。打开另一个命令行窗口，输入如下命令：
