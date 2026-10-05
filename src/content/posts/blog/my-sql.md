---
title: MySQL
published: 2026-10-04
description: 数据库笔记
date: 2026-10-04
updated: 2026-10-04
tags:
  - Java
image: ./cover.jpg
category: Java
draft: false
author: winskyx
---
# MySQL
---

## 为什么要学习数据库

1.岗位需求

2.被迫需求：存储数据

3.**数据库是所有软件体系中最核心的存在** DBA

## 什么是数据库

数据库 (DB,DataBase)

概念：数据仓库，软件，安装在操作系统上

作用：存储数据，管理数据

## 数据库分类

关系型数据库：(SQL)
 - MySQL、Oracle、SQL Server 、 DB2 、 SQL lite
 - 通过表和表之间，行和列之间的关系进行数据的存储；学员信息表，考勤表...

非关系型数据库:   (NoSQL) Not Only
 - Redis，MongDB
 - 非关系型数据库，对象存储，通过对象的自身的属性来决定

DBMS(数据库管理系统)
 - 数据库的管理软件
 - MySQL，数据库管理系统！

## MySQL简介

MySQL是一个关系型数据库管理系统

瑞典MySQL AB公司 --> 属于Oracle旗下产品

MySQL是最好的RDBMS（关系数据库管理系统）应用软件之一

开源

体积小、速度快、总体拥有成本低、找人成本低，所有人必须会

中小型网站、或者大型网站、集群

## 数据库语言

DDL  定义

DML 操作

DQL 查询

DCL  控制

# 操作数据库
---

操作数据库 > 操作数据库中的表 > 操作数据库中表的数据

== mysql 关键字不区分大小写==
## 操作数据库

1. 创建数据库
```sql
CREATE DATABASE [IF NOT EXISTS] westos
```
2. 删除数据库
```sql
DROP DATABASE [IF EXISTS] westos
```
3. 使用数据库
```sql
USE 'school'
```
4. 查询数据库
```sql
SHOW DATABASE -- 查看所有的数据库
```

## 数据库的字段属性（重点）

Unsigned: 
- 无符号的整数
- 声明了该列不能声明为负数

zerofill：
- 0填充的
- 不足的位数，使用0来填充，int（3），5--005

自增：
- 通常理解为自增，自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一的主键 index，必须是整数类型
- 可以自定义设计主键自增的起始值和布长

非空 NULL not null：
- 假设设置为not null，如果不给它赋值，就会报错
- NULL，如果不填写值，默认就是null

默认：
- 设置默认的值
- sex，默认值为男，如果不指定该列的值，则会有默认的值


常用命令

```sql
SHOW CREATE DATABASE school --查看创建数据库的语句
SHOW CREATE TABLE student --查看student数据表的定义语句
DESC student --显示表的结构
```

## 数据表的类型

INNODB   默认使用
MYISAM   早些年使用的


|       | MYISAM | INNODB  |
| ----- | ------ | ------- |
| 事务支持  | 不支持    | 支持      |
| 数据行锁定 | 不支持    | 支持      |
| 外键约束  | 不支持    | 支持      |
| 全文索引  | 支持     | 不支持     |
| 表空间大小 | 较小     | 较大，约为2倍 |

常规使用操作：
- MYISAM 节约空间，速度较快
- INNODB 安全性高，事务的处理，多表用户操作


>在物理空间存在的位置

所有的数据库文件都存在data目录下，一个文件夹就对应一个数据库

本质还是文件的存储

MySQL引擎在物理文件上的区别

- INNODB 在数据库表中只有一个*.frm文件，以及上级目录下的ibdata1文件
- MYISAM对应文件
   -  _*.frm -表结构定义文件
   - _*.MYD 数据文件（data）
   - _*.MYI 索引文件(index)


>设置数据库表的字符集编码

```sql
CAHARSET=utf8
```

若不设置，为MySQL默认的字符集编码Latlin1（不支持中文）

可以在my.ini中配置默认的编码
```xml
character-set-server=utf-8
```



## 修改和删除表

>修改
```sql
--修改表名 ALTER TABLE 旧表名 RENAME AS 新表名
ALTER TABLE teacher RENAME AS teacher1
--增加表的字段 ALTER TABLE 表名 ADD 字段名 列属性
ALTER TABLE teacher1 ADD age INT(11)
--修改表的字段(重命名，修改约束！)
--ALTER TABLE 表名 MODIFY 字段名 列属性
ALTER TABLE teacher1 MODIFY age VARCHAER(11)--修改约束
--ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性
ALTER TABLE teacher1 CHANGE age age1 INT(1)--字段重命名
--删除表的字段  ALTER TABLE 表名 DROP 字段名
ALTER TABLE teacher1 DROP age1

```


| MODIFY | CHANGE                |
| ------ | --------------------- |
| 只能修改约束 | 既可以修改字段名也可以[修改约束](存疑) |


>删除

```sql
--删除表 DROP TABLE IF EXISTS 表名
DROP TABLE IF EXISTS teacher1
```
__所有的创建和删除操作尽量加上判断，以免数据库报错 

## MySQL数据管理
---
### 外键




### DML语言

__数据库意义：数据存储，数据管理

DML语言：数据库操作语言
- Insert
- update
- delete

### 添加

```sql
--INSERT INTO 表名（[字段名1，字段名2，字段名3]）values('值1'，'值2','值3',...)
INSRET INTO `grade` (`gradename`) VALUES(`大四`)
```


```sql 
INSERT INTO `student`(`name`,`pwd`,`sex`) 
VALUES (`李四`,`aaaaaaaa`,`男`)
```

注意事项:
1. 字段和字段之间使用 _英文逗号_隔开
2. 字段是可以省略的，但是后面的值必须要一一对应，不能少
3. 可以同时插入多条数据，VALUES 后面的值，需要使用，隔开即可 ** VALUES(  ),(  )

### 修改

>update 修改谁 (条件)  set 原来的值


```sql
UPDATE `student` SET `name`='winskyx' WHERE id =1;
```

> 不指定条件的情况下，会改动所有的表！

```sql
UPDATE `student` SET name ='黄河'
```

| id  | name |
| --- | ---- |
| 1   | 黄河   |
| 2   | 黄河   |
| 3   | 黄河   |
修改多个属性，用 逗号  (,)  隔开
```sql 
UPDATE `student` SET `name`='黄河',`email`='aigfiaw@qq.com' where id = 1;
```

条件：where子句 运算符 id等于某个值，大于某个值，在某个区间内修改


| 操作符                   | 含义     | 范围          | 结果    |
| --------------------- | ------ | ----------- | ----- |
| =                     | 等于     | 5=6         | false |
| <>或!=                 | 不等于    | 5!6         | true  |
| >                     |        |             |       |
| <                     |        |             |       |
| <=                    |        |             |       |
| >=                    |        |             |       |
| BETWEEN .... AND .... | 在某个范围内 |             |       |
| AND                   | 与&&    | 5>1 AND 1>2 | false |
| OR                    | 或\|\|  | 5>1 OR 1>2  | true  |



```sql
UPDATE 表名 SET column_name= value,[column_name = value,...] where [条件]
```

注意：
- column_name 是数据库的列，尽量带上``
- 条件，筛选的条件，如果没有指定，则会修改所有的列
- value，是一个具体的值，也可以是一个变量
- 多个设置的属性之间，使用英文逗号隔开



语法:
UPDATE 表名 SET column_name = value where 条件[]




### 删除

>delete命令

语法 : delete from 表名



```sql
--删除数据 （避免，会全部删除）
DELETE FROM `student` 
--删除指定数据
DELETE FROM `student` WHERE id =1;
--清空student表
TRUNCATE `student`

```

>delete  的 TRUNCATE区别

- 相同点：都能删除数据，都不会删除表结构
- 不同
 - TRUNCATE 重新设置 自增列 计数器会归零
 - TRUNCATE 不会影响事务

| <center>对比项</center>     | <center>DELETE</center> | <center>TRUNCATE</center> |
| ------------------------ | ----------------------- | ------------------------- |
| 删除范围                     | 可删除部分数据（带 WHERE 条件）     | 全部数据，不支持 WHERE            |
| 是否逐行删除                   | ✅ 是                     | ❌ 否（直接释放数据页）              |
| 是否触发触发器（Trigger）         | ✅ 会触发                   | ❌ 不会触发                    |
| 是否记录事务日志（日志量）            | ✅ 每行记录日志（慢）             | ❌ 只记录数据页释放（快）             |
| 是否可回滚                    | ✅ 是（事务控制）               | ✅ 一般数据库支持（但逻辑不同）          |
| 重置自增主键（如 AUTO_INCREMENT） | ❌ 否                     | ✅ 是，从 1 开始                |
| 是否删除表结构                  | ❌ 否                     | ❌ 否（表结构保留）                |
| 执行速度                     | 慢（逐行）                   | 快（批量释放）                   |

# DQL查询数据（最重点）
---
## DQL
(Data Query Language：数据查询语言)
- 所有的查询都用它 Select
- 简单的查询与复杂查询都可
- 数据库中最核心的语言，最重要的语句
- 使用频率最高的语句

### 指定查询字段

```sql
--查询全部的学生 SELECT 字段 FROM 表
SELECT * FROM student

--查询指定字段
SELECT `StudentNo`,`StudentName` FROM student

-- 别名，给结果起一个名字 AS 可以给字段起别名，也可以给表起别名

Select `StudentNo` AS 学号,`StudentName` AS学生姓名 From student AS s

-- 函数 Concat(a,b)
SELECT CONCAT('姓名:',StudentName) AS 新名字 From student
```


> 有的时候，列的名字不是那么见名知意，我们起别名 AS，{ 字段名   AS    别名}


>去重 distinct

作用：取出SELECT查询结果中重复的数据，重复的数据只显示一条

```sql
--查询一下有哪些同学参加了考试，成绩
SElECT * from result --查询全部的考试成绩

select `StudentNo` from result --查询有哪些同学参加了考试

--发现重复数据，去重
select distinct `StudentNo` From result

--成绩+1查看
Select `studentno`,`studentresult`+1 as `提分后` from result

```

数据库中的表达式：文本值，列，null，函数，计算表达式，系统变量

```SQL
SELECT 表达式 FROM 表
```


## where 条件子句

作用：检索数据中 符合条件 的值
搜索的条件由一个或者多个表达式组成！结果  布尔值


>逻辑运算符


| 运算符      | 语法                 | 描述               |
| -------- | ------------------ | ---------------- |
| and &&   | a and b   a && b   | 逻辑与，两个都为真，结果为真   |
| or \|\|  | a or b      a\|\|b | 逻辑或，其中一个为真，则结果为真 |
| Not    ! | not a       !a     | 逻辑非              |
|          |                    |                  |

尽量使用英文字母

## 模糊查询

>模糊查询：比较运算符


| 运算符         | 语法                 | 描述                          |
| ----------- | ------------------ | --------------------------- |
| IS NULL     | a is null          | 如果操作符为NULL，结果为真             |
| IS NOT NULL | a is not null      | 如果操作符不为NULL，结果为真            |
| BETWEEN     | a between b and c  | 若a在b和c之间，则结果为真              |
| LIKE        | a like b           | SQL匹配，如果a匹配b，则结果为真          |
| IN          | a in (a1,a2,a3...) | 假设a在a1，或者a2...其中的某一个值中，结果为真 |
|             |                    |                             |
|             |                    |                             |

__Like模糊查询

```sql
Select * from emp where ename like 'M%';
```

查询 EMP 表中 Ename 列中有 M 的值，M 为要查询内容中的模糊信息。
- **%** 表示多个字值，  *_ 下划线表示一个字符；
- **M%** : 为能配符，正则表达式，表示的意思为模糊查询信息为 M 开头的。
- **%M%** : 表示查询包含M的所有内容。
- **%M_** : 表示查询以M在倒数第二位的所有内容。


## 联表查询



