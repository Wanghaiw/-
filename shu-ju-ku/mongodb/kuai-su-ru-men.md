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
'D:\MongoDb\bin\mongo.exe'
行些命令后，就能连接上MongoDB服务。由于没有配置任何其他端口，也没有配置权限认证，所以一切都是默认的本地连接，相当简单。连接成功后，可以执行help命令，看看有什么内容。

## 安装MongoDB服务
在没有安装服务之前，每次使用都需要先手动打开mongodb服务,才能连接。我们可以直接把MongoDB的服务安装到计算机,启动之后就不用每次手动打开服务了。
过执行mongod.exe，使用--install选项来安装服务，使用--config选项来指定之前创建的配置文件。
`"D:\MongoDB\bin\mongod.exe" --config "D:\MongoDB\mongod.cfg" --install`
安装完成之后通过`net start MongoDB`,启动服务
`net stop MongoDB` 关闭服务


