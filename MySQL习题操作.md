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
create table SC(SId varchar(1；0),CId varchar(10),score decimal(18,1));
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




#### 查询"01"课程比"02"课程成绩高的学生的信息及课程分数

##### 第一种方法
直接使用select-from-where，只是where的条件较多
```
select st.*,
    -> s1.score '01_score',
    -> s2.score '02_score'
    -> from
    -> student st,
    -> sc s1,
    -> sc s2
    -> where
    -> st.sid = s1.sid
    -> and st.sid = s2.sid
    -> and s1.cid = '01'
    -> and s2.cid = '02'
    -> and s1.score > s2.score;
```
##### 第二种方法
使用left join和right join
```
select st.*,
    -> sc1.score '01_score',
    -> sc2.score '02_score'
    -> from
    -> student st
    -> left join sc sc1 on st.sid=sc1.sid
    -> and sc1.cid ='01'
    -> left join sc sc2 on st.sid=sc2.sid
    -> and sc2.cid ='02'
    -> where
    -> sc1.score>sc2.score;
```

#### 查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩
```
 SELECT
    -> st.sid,
    -> st.sname,
    -> ROUND(( sc.score ),2) avg_score
    -> from
    -> Student st
    -> left join sc
    -> on st.sid =sc.sid
    -> group
    -> by
    -> sc.sid
    -> having
    -> AVG(sc.score)>60;
```

#### 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩
```
 select
    -> st.sid,
    -> st.sname,
    -> count(sc.cid) course_number,
    -> sum(sc.score) score_sum
    -> from
    -> student st
    -> left join
    -> sc
    -> on st.sid =sc.sid
    -> group by st.sid;
```

#### 查询“李”姓老师的数量
```
 select
    -> teacher.tname,count(teacher.tname) name_li_number
    -> from teacher
    -> where
    -> teacher.tname like '李%';
```

#### 查询学过张三老师授课的同学的信息
```
select
    -> st.*
    -> from
    -> student st,
    -> course c,
    -> sc,
    -> teacher t
    -> where
    -> st.sid = sc.sid
    -> and sc.cid = c.cid
    -> and c.tid = t.tid
    -> and t.tname = '张三';

```

查询学过编号为01，并且学过编号为02课程的学生信息
```
SELECT
	st.* 
FROM
	Student st,
	Score s1,
	Score s2 
WHERE
	st.s_id = s1.s_id 
	AND s1.s_id = s2.s_id 
	AND s1.c_id = "01" 
	AND s2.c_id = "02"
```

























