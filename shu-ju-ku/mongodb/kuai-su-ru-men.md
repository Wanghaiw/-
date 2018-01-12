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

