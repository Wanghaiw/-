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
```
# 查看数据表
语法 `SHOW TABLES FROM DATABASE`,不加from database 查看当前数据库下面的表，加了之后查看指定的数据库。

## 查看数据表结构
语法 `SHOW COLUMNS FROM TABLE_NAME`

`show columns from students` 查看刚刚创建的students表结构。
