# 概述
数据表是数据库中最重要的组成部分之一，是其他对象的基础。

# 创建数据表
`CREATE TABLE [IF NOT EXISTS] table_name(column_name data_type,.....)`

例如创建一张学生表，里面有name和age两个字段。
```
create table if not exists students(
    name varchar(10),
    age int
    );