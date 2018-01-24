# 什么是Mysql数据库
MySQL是一个关系型数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle公司。MySQL是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。
* Mysql是开源的，所以你不需要支付额外的费用。
* Mysql支持大型的数据库。可以处理拥有上千万条记录的大型数据库。
* MySQL使用标准的SQL数据语言形式。
* Mysql可以允许于多个系统上，并且支持多种语言。这些编程语言包括C、C++、Python、Java、Perl、PHP、Eiffel、Ruby和Tcl等。
* MySQL支持大型数据库，支持5000万条记录的数据仓库，32位系统表文件最大可支持4GB，64位系统支持最大的表文件为8TB。

# 安装Mysql
* 1.下载mysql5.6的zip文件
* 2.把zip文件解压到D:\mysql-server
* 3.配置环境变量
* 4.打开mysql的配置文件(.ini后缀的文件)，修改里面的basedir和datadir 分别设置为D:\mysql-server和D:\mysql-server\data.
* 5.用管理员身份运行cmd,cd到mysql下面的bin目录,输入mysqld -install。
* 6. net start mysql 启动服务
* 7. mysql -u root -p 进入mysql环境，默认没有设置密码。

# 操作数据库

## 创建数据库
语法 CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name

`CREATE DATABASE IF NOT EXISTS tz_mysql CHARACTER SET utf8;` 创建tz_mysql数据库，默认编码为latin1。

`show CREATE DATABASE tz_mysql;` 可以查看创建数据库的命令。

`show variables like 'character_set_database';` 查看当前数据库的编码。

`alter database tz_mysql character set = utf8;` 设置tz_mysql的编码为utf8，。