#### MySQL 简单入门

##### 验证是否安装成功

安装部分从官网下载安装，判断安装是否成功可以使用命令

```
mysqladmin --version
```

我安装的版本是

```
mysqladmin  Ver 8.0.22 for Win64 on x86_64 (MySQL Community Server - GPL)
```

在进入mysql的时候，出现一个error
```
ERROR 1045 (28000): Access denied for user 'ODBC'@'localhost' (using password: NO)
```
解决方案如下：

MySQL的默认用户是ODBC，即使在安装过程中没有创建这个用户，因为在登入mysql的时候使用的用户是还未创建的ODBC，因此会报该错误。
解决方案就是登入账户的时候使用具体的用户名，命令如下
```
mysql -u root -p
```
-u 指定用户名 -p 在命令行提示符的情况下输入密码

该root用户是无密码的，需要进行如下设置root密码
```
mysqladmin -u root password "123456"
```

关闭MySQL服务器

```
mysqladmin -u root -p shutdown
```

设置MySQL用户账户

只要在数据库中的新纪录添加到用户表mysql.user

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
