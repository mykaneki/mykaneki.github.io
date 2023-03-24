---
title: "SQL三表联查、分组去重"
date: 2023-01-07
tags: ["sql"]
---

表结构

![image-20220410173624166](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241405630.png)![image-20230324140520329](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241405408.png)

- 查询选修了“苏亚步”教师开设课程的所有学生的学号和姓名

  ![image-20230324140533229](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241405288.png)

- 统计每一年龄选修课程的学生人数

  ![image-20230324140540303](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241405361.png)

### date如何输入数据

按照格式，以字符串形式输入

```sql
create table students(
	学号 int not null primary key,
	姓名 varchar(20),
	性别 varchar(5),
	年龄 int,
	所在院系 varchar(50),
	班级名 varchar(50),
	入学日期 date,
);

insert into students values(20009001,'葛文卿','女',22,'国际贸易','国贸2班','20000829'),
							(20014019,'郑秀丽','女',21,'会计学','会计1班','20010902'),
							(20023001,'刘成铠','男',18,'计算机','软件2班','20020827'),
							(20026002 ,'李涛','女',19,'电子学','电子1班','20020827'),
							(20023002 ,'沈香娜','女',19,'计算机','软件2班','20020827'),
							(20026003 ,'李涛','男',19,'计算机','软件1班','20020827'),
							(20023003 ,'肖一竹','女',19,'计算机','软件2班','20020827');

```
