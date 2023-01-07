---
title: "SQL三表联查、分组去重"
date: 2023-01-07
tags: ["sql"]
---

表结构

![image-20220410173604674](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204101736765.png)![image-20220410173624166](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204101736253.png)![image-20220410173703000](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204101737074.png)

- 查询选修了“苏亚步”教师开设课程的所有学生的学号和姓名

  ![img](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204101737176.png)

- 统计每一年龄选修课程的学生人数

  ![img](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202204101737159.png)

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
