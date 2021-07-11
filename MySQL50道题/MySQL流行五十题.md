#### 概述
该数据库一共有四张表。

第一张为学生表，结构如下：

**Student (s_id, s_name, s_birth, s_sex)**

如图：

![students](students.png)


第二张为课程表

**Course(c_id, c_name, t_id)**

![course](course.png)


第三张为教师表

**Teacher(t_id, t_name)**

![teachers](teachers.png)


第四张为成绩表

**Score(s_id, c_id, score)**

![score](score.png)


****

#### 建表
```SQL
# 建表
# 学生表
CREATE TABLE `Student`(
    `s_id` VARCHAR(20),
    `s_name` VARCHAR(20) NOT NULL DEFAULT '',
    `s_birth` VARCHAR(20) NOT NULL DEFAULT '',
    `s_sex` VARCHAR(10) NOT NULL DEFAULT '',
    PRIMARY KEY(`s_id`)
);
# 课程表
CREATE TABLE `Course`(
    `c_id`  VARCHAR(20),
    `c_name` VARCHAR(20) NOT NULL DEFAULT '',
    `t_id` VARCHAR(20) NOT NULL,
    PRIMARY KEY(`c_id`)
);
# 师表
CREATE TABLE `Teacher`(
    `t_id` VARCHAR(20),
    `t_name` VARCHAR(20) NOT NULL DEFAULT '',
    PRIMARY KEY(`t_id`)
);
# 成绩表
CREATE TABLE `Score`(
    `s_id` VARCHAR(20),
    `c_id`  VARCHAR(20),
    `s_score` INT(3),
    PRIMARY KEY(`s_id`,`c_id`)
);
# 插入学生表测试数据
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('08' , '王菊' , '1990-01-20' , '女');
#课程表测试数据
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');

# 教师表测试数据
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');

#成绩表测试数据
insert into Score values('01' , '01' , 80);
insert into Score values('01' , '02' , 90);
insert into Score values('01' , '03' , 99);
insert into Score values('02' , '01' , 70);
insert into Score values('02' , '02' , 60);
insert into Score values('02' , '03' , 80);
insert into Score values('03' , '01' , 80);
insert into Score values('03' , '02' , 80);
insert into Score values('03' , '03' , 80);
insert into Score values('04' , '01' , 50);
insert into Score values('04' , '02' , 30);
insert into Score values('04' , '03' , 20);
insert into Score values('05' , '01' , 76);
insert into Score values('05' , '02' , 87);
insert into Score values('06' , '01' , 31);
insert into Score values('06' , '03' , 34);
insert into Score values('07' , '02' , 89);
insert into Score values('07' , '03' , 98);

```

###### 1、查询“01”课程比“02”课程成绩高的学生的信息以及课程分数
第一种方法——自连接
```SQL
select 
    c.*,
    a.s_score s01,
    b.s_score s02
from 
    score a, 
    score b, 
    student c
where 
    a.c_id = '01'
and 
    b.c_id = '02'
and 
    a.s_id = b.s_id
and 
    a.s_id = c.s_id
and 
    a.s_score > b.s_score;
```
第二种方法——长型数据变为宽型数据
```SQL
select 
    s.*,
    t.s01, 
    t.s02
from
    (select 
        a.s_id,
        max(case when a.c_id = '01' then a.s_score end) s01,
        max(case when a.c_id = '02' then a.s_score end) s02
    from 
        score a
    group by
        a.s_id) t, 
    Student s
where 
    t.s01 > t.s02
and 
    t.s_id = s.s_id;
```

****

###### 2、查询“01”课程比“02”课程成绩低的学生的信息以及课程分数
```SQL
select 
    s.*,
    t.s01, 
    t.s02
from
    (select 
        a.s_id,
        max(case when a.c_id = '01' then a.s_score end) s01,
        max(case when a.c_id = '02' then a.s_score end) s02
    from 
        score a
    group by
        a.s_id) t, 
    Student s
where 
    t.s01 < t.s02
and 
    t.s_id = s.s_id;
```

****

###### 3、查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩
子查询的方法
```SQL
select 
    s.s_id, 
    s.s_name, 
    t.score
from
    (select 
        s_id, 
        avg(s_score) score
    from 
        score
    group by 
        s_id) t, 
    student s
where
    t.s_id = s.s_id
and 
    t.score > 60;
```
两个表连接的方法
```SQL
select
    a.s_id, 
    s.s_name,
    avg(a.s_score) avg_s
from
    score a, 
    student s
where 
    a.s_id = s.s_id
group by 
    a.s_id
having 
    avg(a.s_score) >= 60;
```
****

###### 4、查询平均成绩小于60分的同学的学生编号和学生姓名和平均成绩
```SQL
select
    a.s_id, 
    s.s_name,
    avg(a.s_score) avg_s
from
    score a, 
    student s
where 
    a.s_id = s.s_id
group by 
    a.s_id
having 
    avg(a.s_score) < 60;
```
****

###### 5、查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩

```SQL
select
    s.s_id,
    s.s_name,
    count(a.c_id) cnt_s, 
    ifnull(sum(a.s_score), 0) sum_s
from 
    score a
right join 
    student s
on 
    a.s_id = s.s_id
group by 
    a.s_id;
```

****

###### 6、查询“李”姓老师的数量
```SQL
select count(*) cnt_t
from teacher t
where t.t_name like '李%';
```

****

###### 7、查询学过张三老师授课的学生的信息
```SQL
select 
    c.*
from 
    course a, 
    score b, 
    student c, 
    teacher d
where 
    d.t_id = a.t_id
and 
    a.c_id = b.c_id
and 
    b.s_id = c.s_id
and 
    d.t_name = '张三';
```

****

###### 8、查询没学过张三老师授课的学生的信息
```SQL
select 
    * 
from
    student s
where 
    s.s_id 
not in
    (select 
        b.s_id
    from 
        course a, 
        score b, 
        teacher d
    where 
        d.t_id = a.t_id
    and 
        a.c_id = b.c_id
    and 
        d.t_name = '张三');
```

****

###### 9、查询学过编号为“01”并且也学过编号为“02”的课程的同学的信息

```SQL
select 
    c.*
from 
    score a, 
    score b, 
    student c
where 
    a.c_id = '01' 
and 
    b.c_id ='02'
and 
    a.s_id = b.s_id
and 
    a.s_id = c.s_id;
```

****

###### 10、查询学过编号为“01”但是没有学过编号为“02”的课程的同学的信息
```SQL
select 
    s.* 
from
    (select 
        a.s_id,
        max(case when a.c_id = '01' then a.s_score end) s01,
        max(case when a.c_id = '02' then a.s_score end) s02
    from 
        score a
    group by
        a.s_id) t, student s
where
    t.s_id = s.s_id
and 
    t.s01 is not null
and 
    t.s02 is null;
```

****

###### 11、查询没有学全所有课程的同学的信息
```SQL
select 
    s.* 
from
    (select 
        a.s_id,
        max(case when a.c_id = '01' then a.s_score end) s01,
        max(case when a.c_id = '02' then a.s_score end) s02,
        max(case when a.c_id = '03' then a.s_score end) s03
    from 
        score a
    group by
        a.s_id
    having 
        s01 is null 
    or
        s02 is null 
    or
        s03 is null) t, 
        student s
where 
    s.s_id = t.s_id;
```

****

###### 12、查询至少有一门课与学号01的同学所学相同的同学的信息

```SQL
select
    a.*
from 
    student a
left join
    score b
on 
    a.s_id = b.s_id
where 
    b.c_id 
in (select 
        c_id 
    from 
        score 
    where 
        s_id = '01')
group by 
    1,2,3,4;
```
****


###### 13、查询和01同学所学完全相同的同学的信息
```SQL
create table s01_s_temp as
select
    t.*, b.c_id c_id2
from
    (select
        a.*, 
        b.c_id
    from
        student a, 
        (select 
            c_id 
        from 
            score 
        where 
            s_id ='01') b) t
left join
    score b
on 
    t.s_id = b.s_id
and 
    t.c_id = b.c_id

union

select
    t.*, b.c_id c_id2
from
    (select
        a.*, 
        b.c_id
    from
    student a, 
    (select 
        c_id 
    from 
        score 
    where 
        s_id ='01') b) t
right join
    score b
on 
    t.s_id = b.s_id
and 
    t.c_id = b.c_id;

-------------------------------------------
select 
    * 
from 
    student 
where 
    s_id 
not in
    (select 
        s_id 
    from 
        s01_s_temp
    where 
        c_id2 is null 
    or 
        c_id is null)
and 
    s_id != '01';
```

****

###### 14、查询没学过张三老师讲授的课的学生姓名
```SQL
select 
    * 
from
    student s
where 
    s.s_id 
not in
    (select 
        b.s_id
    from 
        course a, 
        score b, 
        teacher d
    where 
        d.t_id = a.t_id
    and 
        a.c_id = b.c_id
    and 
        d.t_name = '张三');
```

****

###### 15、查询两门及其以上不及格的课程的同学的学号、姓名及其平均成绩

```SQL
select 
    a.s_id, 
    a.s_name, 
    avg(b.s_score) avg_s
from 
    student a
left join
    score b
on 
    a.s_id = b.s_id
group by 
    a.s_id
having 
    sum(case when b.s_score >= 60 then 0 else 1 end) >= 2;
```

**** 

###### 16、检索01课程分数小于60，按分数降序排列的学生信息

```SQL
select 
    a.*, 
    b.s_score
from 
    student a
left join
    score b
on 
    a.s_id = b.s_id
where 
    b.c_id = '01'
and 
    b.s_score < 60
order by
    s_score desc;

```

****

###### 17、按照平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

```SQL
select 
    * 
from
    (select 
        * 
    from score) a1,
    (select 
        a.s_id, 
        round(avg(s_score), 2) avg_s 
    from 
        score a 
    group by s_id) a2
where 
    a1.s_id = a2.s_id
order by
    avg_s desc;
```

开窗函数
```SQL
#8.0以上的版本
select
    a.*,
    avg(s_score) over(partition by a.s_id) avg_s
from 
    score a;
```

****

###### 18、查询各科成绩最高分最低分和平均分

```SQL
select 
    a.c_id,
    a.c_name, 
    max(s_score) max_s,
    min(s_score) min_s, 
    avg(s_score) avg_s,
    sum(case when s_score >= 60 then 1 else 0 end) / count(1) jige,
    sum(case when s_score >= 70 and s_score < 80 then 1 else 0 end) / count(1) zhongdeng,
    sum(case when s_score >= 80 and s_score < 90 then 1 else 0 end) / count(1) youliang,
    sum(case when s_score >= 90 then 1 else 0 end) / count(1) youxiu
from 
    course a
left join
    score b
on 
    a.c_id = b.c_id
group by    
    a.c_id;
```

****

###### 19、按照各科成绩进行排序并显示排名

```SQL
#8.0以后的版本
select
    a.*,
    rank() over(partition by c_id order by s_score desc) rk
from
    score a;
``` 

****

###### 20、查询学生的总成绩并进行排名

```SQL
create table sum_s_temp as 
    select
        s_id,
        sum(s_score) sum_s
    from 
        score
    group by
        s_id;

select * from sum_s_temp;

select 
    a.*,
    (select 
        count(sum_s) from sum_s_temp b 
    where 
        a.sum_s < b.sum_s) + 1 rk
from 
    sum_s_temp a
order by rk;
```

****

###### 21、查询不同老师所教不同课程平均分

```SQL
select
    a.t_name, b.c_name, avg(s_score) avg_s
from
    teacher a
left join
    course b
on 
    a.t_id = b.t_id
left join
    score c
on 
    b.c_id = c.c_id
group by
    a.t_name
order by 
    avg_s desc;
```

****

###### 22、查询所有课程第二名到第三名的学生信息及该课程成绩

```SQL
select
    a.*, t.c_id, t.s_score
from
    student a
left join
    (select
        a.*, 
        rank() over (order by s_score desc) rk
    from 
        score a) t
on 
    a.s_id = t.s.s_id
where 
    rk in (2, 3);
```

****

###### 23、统计各科成绩各个分数段人数

```SQL
select
    a.c_id, a.c_name,
    sum(case when s_score > 85 and s_score <= 100 then 1 else 0 end) "[85-100]",
    sum(case when s_score > 70 and s_score <= 85 then 1 else 0 end) "[70-85]",
    sum(case when s_score > 60 and s_score <= 70 then 1 else 0 end) "[60-70]",
    sum(case when s_score > 0 and s_score <= 60 then 1 else 0 end) "[0-60]"
from 
    course a
left join
    score b
on 
    a.c_id = b.c_id
group by
    a.c_id, a.c_name;
```

****

###### 24、查询学生平均成绩及其名次

```SQL

select
    t.*, 
    rank() over(order by avg_s desc) rk
from
    (select
        s_id,
        avg(s_score) avg_score
    from 
        score
    group by
        s_id) t;
```

****

###### 25、查询各科成绩前三名的记录

```SQL
select
t.*
from
(select
    a.c_id, 
    a.c_name, 
    b.s_score,
    rank() over(partition by a.c_id order by s_score desc) rk
from 
    course a
left join
    score b
on 
    a.c_id = b.c_id
order by
    a.c_id, b.s_score desc) t
where rk in (1, 2, 3);
```

****

###### 26、查询每门课程被选修的学生数

```SQL
select 
    a.c_id, 
    a.c_name, 
    count(b.s_id) cnt
from
    course a
left join
    score b
on 
    a.c_id = b.c_id
group by 
    a.c_id;
```

****

###### 27、查询只有两门课程的全部学生的学号和姓名

```SQL
select
    a.s_id,
    a.s_name
from
    student a
left join
    score b
on 
    a.s_id = b.s_id
group by 
    a.s_id
having 
    count(c_id) = 2;
```

****

###### 28、查询男女人数

```SQL
select
    s_sex, 
    count(s_id) cnt
from
    student a
group by
    a.s_sex;
```

****

###### 29、查询名字中含有风的学生信息
```SQL
select
    *
from
    student
where
    s_name like '%风%';
```

****

###### 30、查询同名同姓学生名单

```SQL
select
    s_name, 
    s_sex, 
    count(s_name) cnt
from 
    student
group by
    s_name
having
    count(s_name) > 1;
```

****

###### 31、查询1990年出生的学生名单

```SQL
select
    *
from 
    student
where 
    year(s_birth) = 1990;
```

****

###### 32、查询每门课程平均成绩

```SQL
select
    c_id, 
    avg(s_score) avg_s
from
    score
group by
    c_id
order by
    avg_s desc, 
    c_id asc;
```

****

###### 33、查询平均成绩大于等于85的学生信息

```SQL
select 
    a.s_id, 
    a.s_name,
    ifnull(avg(s_score), 0) avg_s
from
    student a
left join
    score b
on 
    a.s_id = b.s_id
group by
    a.s_id
having
    ifnull(avg(s_score), 0) >= 85;
```

****

###### 34、查询课程名为数学，并且分数低于60的学生姓名和分数

```SQL
select
a.s_name, b.s_score
from student
left join score b
on a.s_id = b.s_id
left join course c
on b.c_id = c.c_id
where
c.c_name = '数学`
and s_score < 60;
```

****

###### 35、查询所有学生的课程及分数情况

```SQL
select
    a.s_name, 
    c.c_name, 
    b.s_score
from
    student a
left join
    score b
on 
    a.s_id = b.s_id
left join
    course c
on 
    c.c_id = b.c_id;
```

****

###### 36、查询每门课程成绩都在70分以上的姓名、课程名称和分数

```SQL
select
    a.s_name, 
    c.c_name, 
    b.s_score
from 
    student a
left join
    score b
on 
    a.s_id = b.s_id
left join
    course c
on 
    c.c_id = b.c_id
where 
    a.s_id in (
select s_id from score group by s_id having min(s_score) > 70);
```

****

###### 37、查询不及格的课程

```SQL
select
    b.s_id, 
    a.c_name
from
    course a
left join
    score b
on 
    a.c_id = b.c_id
where
    s_score < 60;
```

****

###### 38、查询课程编号为01，且课程成绩在80分以上的学生的学号与姓名

```SQL
select
    a.s_id, 
    a.s_name
from
    student a
left join
    score b
on 
    a.s_id = b.s_id
where
    b.c_id = '01'
and 
    s_score >= 80;
```


****

###### 39、求每门课程的人数

```SQL
select
    a.c_name, 
    count(b.s_id) cnt
from 
    course a
left join
    score b
on 
    a.c_id = b.c_id
group by 
    a.c_id;
```

****

###### 40、查询选修张三老师所授课程的学生中，成绩最高的学生信息及其成绩

```SQL
select
    a.*, 
    c.c_name, 
    b.s_score
from
    student a
left join
    score b
on 
    a.s_id = b.s_id
left join
    course c
on 
    c.c_id = b.c_id
left join
    teacher d
on 
    d.t_id = c.t_id
where 
    d.t_name =  '张三'
order by
    s_score desc
limit 1;
```

****

###### 41、查询不同课程成绩相同的学生编号、学生成绩

```SQL
select
distinct a.*
from
score a, score b
where a.c_id != b.c_id and a.s_score = b.s_score;
```
****

###### 42、查询每门成绩最好的前两名

```SQL
--开窗函数
select 
    * 
from 
    (select
        a.c_id, 
        a.s_score, 
        rank() over(partition by c_id order by s_score desc) rk
    from score a) t
where 
    rk <= 2

--子查询
select
    a.c_id, 
    a.s_score
from
    score a
where 
    (select 
        count(s_score) 
    from 
        score b 
    where 
        a.c_id = b.c_id 
    and a.s_score < b.s_score) + 1 <= 2
order by c_id, s_score desc;
```

****

###### 43、统计每门课程的选修人数，超过五人才统计

```SQL
select 
    a.c_id,
    count(s_id) cnt
from
    course a
left join
    score b
on 
    a.c_id = b.c_id
group by 
    a.c_id
having 
    count(s_id) >= 5
order by 
    cnt desc, a.c_id asc;
```

****

###### 44、检索至少选修两门课程的学生的学号

```SQL
select
    s_id
from
    score
group by
    s_id
having
    count(c_id) >= 2;
```

****

###### 45、查询选修了全部课程的学生信息

```SQL

select 
    a.* 
from
    student a,
    (select
        s_id
    from
        score
    group by
        s_id
    having
        count(c_id) = (select 
                            count(1) 
                        from 
                            course)) b
where a.s_id = b.s_id;
```

****

###### 46、查询各学生的年龄

```SQL
select 
    a.*,
    year(now()) - year(s_birth) age 
from 
    student a;
```
****

###### 47、查询本周过生日的学生

```SQL
select 
    a.*
from 
    student a
where
    weekofyear(str_to_date(concat(year(now()), date_format(s_birth, '%m%d')), '%y%m%d')) = weekofyear(now());
```

****

###### 48、查询下周过生日的学生

```SQL
select 
    a.*
from 
    student a
where
    weekofyear(str_to_date(concat(year(now()), date_format(s_birth, '%m%d')), '%y%m%d')) = weekofyear(now() + interval '7' day);
```

****

###### 49、查询本月过生日的学生

```SQL
select 
    a.*
from 
    student a
where
    month(now()) = month(s_birth);
```

****

###### 50、查询下月过生日的学生

```SQL
select 
    a.*
from 
    student a
where
    month(now() + interval '1' month) = month(s_birth);
```

****