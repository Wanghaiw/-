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

# 插入数据
语法 `INSERT TABLE_NAME [(col_name,...)] VALUE (val,....)`
`insert students value('韶辉',25)` 在students数据表插入一条数据。

# 查找数据
语法 `SELECT expr,... FROM TABLE_NAME WHERE 条件`
`select * from students` 查看students表所有字段的数据 *可以换成字段名。

# 删除数据
语法 `DELETE FROM TABLE WHERE 条件`
`delete from cs where id =0;`
删除cs表里面 id为0的数据。

# 主键 PRIMARY KEY
* 主键约束
* 每张数据表只能存在一个主键
* 主键保证记录的唯一性
* 主键自动为NOT NULL

```
create table cs (
id int primary key, # 设置id为主键
name varchar(10) not null # name 字段不能为空
);
```



