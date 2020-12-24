#### 测试表格

##### 学生表
Student(S#,Sname,Sage,Ssex)
- S# 学生编号
- Sname 学生姓名
- Sage 出生年月
- Ssex 学生性别

##### 课程表
Course(C#,Cname,T#)
- C# 课程编号
- Cname 课程名称
- T# 教师编号
##### 教师表
Teacher(T#,Tname)
- T# 教师编号
- Tname 教师姓名
##### 成绩表
SC(S#,C#,score)
- S# 学生编号
- C# 课程编号 
- score 分数

#### 创建测试数据

##### 学生表

###### 创建学生表

```
CREATE table Student
(SId varchar(10),Sname varchar(10),Sage datetime,Ssex varchar(10));
```

###### 插入数据
```
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');

insert into Student values('02' , '钱电' , '1990-12-21' , '男');

insert into Student values('03' , '孙风' , '1990-12-20' , '男');

insert into Student values('04' , '李云' , '1990-12-06' , '男');

insert into Student values('05' , '周梅' , '1991-12-01' , '女');

insert into Student values('06' , '吴兰' , '1992-01-01' , '女');

insert into Student values('07' , '郑竹' , '1989-01-01' , '女');

insert into Student values('09' , '张三' , '2017-12-20' , '女');

insert into Student values('10' , '李四' , '2017-12-25' , '女');

insert into Student values('11' , '李四' , '2012-06-06' , '女');

insert into Student values('12' , '赵六' , '2013-06-13' , '女');

insert into Student values('13' , '孙七' , '2014-06-01' , '女');
```
##### 课程表

###### 创建课程表

一般来说，如果含有中文字符，用nchar/nvarchar，如果纯英文和数字，用char/varchar。


```
create table Course(CId varchar(10),Cname nvarchar(10),TId varchar(10));
```

###### 插入数据

```
insert into Course values('01' , '语文' , '02');

insert into Course values('02' , '数学' , '01');

insert into Course values('03' , '英语' , '03');
```
##### 教师表
###### 创建教师表
```
create table Teacher(TId varchar(10),Tname varchar(10));
```
###### 插入数据
```
insert into Teacher values('01' , '张三');

insert into Teacher values('02' , '李四');

insert into Teacher values('03' , '王五');
```
##### 分数表
###### 创建分数表
```
create table SC(SId varchar(10),CId varchar(10),score decimal(18,1));
```
###### 插入数据
```
insert into SC values('01' , '01' , 80);

insert into SC values('01' , '02' , 90);

insert into SC values('01' , '03' , 99);

insert into SC values('02' , '01' , 70);

insert into SC values('02' , '02' , 60);

insert into SC values('02' , '03' , 80);

insert into SC values('03' , '01' , 80);

insert into SC values('03' , '02' , 80);

insert into SC values('03' , '03' , 80);

insert into SC values('04' , '01' , 50);

insert into SC values('04' , '02' , 30);

insert into SC values('04' , '03' , 20);

insert into SC values('05' , '01' , 76);

insert into SC values('05' , '02' , 87);

insert into SC values('06' , '01' , 31);

insert into SC values('06' , '03' , 34);

insert into SC values('07' , '02' , 89);

insert into SC values('07' , '03' , 98);
```















































