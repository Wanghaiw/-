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
### 模糊查询
通配符：
* 1.% 表示任意字符，课匹配任意类型和长度的字符。
* 2._ 表示任意单个字符。常用来限制字符长度。
* 3.[] 表示括号内所列字符中的一个。
* 4. [^] 表示不在括号内的单个字符。
语法 `SELECT expr,.. FROM TABLE WHERE expr LIKE 条件`
` select * from cs where name like '%云';`

# 修改数据
语法 `UPDATE TABLE SET 字段名='内容' [WHERE '条件']`
`update student set sex ='男' where id=2;`
## 修改表名
语法 `ALERT TABLE 旧表名 RENAME 新表名`

## 修改字段名
语法 `ALTER TABLE 表名 CHANGE 原字段名 新字段名 字段类型 约束条件`
`alter table student change name username varchar(10);`

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



