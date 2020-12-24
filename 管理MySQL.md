#### MySQL管理

##### 启动和关闭MySQL服务器
- 启动
```
mysqld --console
```
- 关闭
```
mysqladmin -u root shutdown
```

##### MySQL用户设置
要添加MySQL用户只需要在MySQL数组库中的user表添加新用户即可。

**添加新用户方法**如下
```
//使用root登入MySQL
 mysql -u root -p
//切入名为mysql的数据库
use mysql
//在用户表中插入用户
select host,user,authentication_string from mysql.user;
create user 'ccc'@'localhost' identified by '123456';
//创建数据库
create database 数据库名
```

##### 管理MySQL的命令
- USE 数据库名;
选择要操作的数据库
- SHOW DATABASES;
列出MySQL数据库管理系统的数据库列表
- SHOW TABLES;
显示指定数据库的所有表，使用该命令前需要使用use命令来选择要操作的数据库。
- SHOW COLUMNS FROM 数据表
显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。
- SHOW INDEX FROM 数据表;
显示数据表的详细索引信息，包括PRIMARY KEY（主键）。
- SHOW TABLE STATUS;
该命令将输出Mysql数据库管理系统的性能及统计信息。