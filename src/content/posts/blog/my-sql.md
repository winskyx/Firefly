---
title: MySQL
published: 2026-10-04
description: 数据库笔记
date: 2026-10-04
updated: 2026-10-13
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



__7种Join理论

![](https://i-blog.csdnimg.cn/blog_migrate/93f13cb652b17d4ef93703fd8e9f9a88.png)



思路：
1.分析查询的字段来自哪些表(连接查询)
2.确定使用哪种连接查询？7种
确定交叉点（这两个表中哪个数据是相同的）
判断的条件：学生表中的studentNo = 成绩表 studentNo

-- join （连接的表） on（判断的条件） 连接查询
--where 等值查询

| 操作         | 描述                    |
| ---------- | --------------------- |
| Inner join | 如果表中有一个匹配，就返回行        |
| Left join  | 会从左表中返回所有的值，即使右表中没有匹配 |
| Right join | 会从右表中返回所有的值，即使左表中没有匹配 |



>自连接

自己的表和自己的表连接，核心：一张表拆为两张一样的表即可

## 分页和排序


排序 : __order by 字段名 DESC(降序) || ASC （升序）

分页：__limit 第几条数据开始，每页多少条

-- LIMIT    0,5    1~5
-- LIMIT    1,5    2~5

例 : __Limit 1,5

## 子查询

```sql
SELECT s.studentno ,studentname
FROM student s
---
WHERE studentno IN(
	SELECT studentno 
	FROM result 
	WHERE studentresult >= 80
)
---
```

## 分组和过滤

Group by [字段] ---分组
having 条件   - -- 过滤

```sql
-- 顺序
(7) SELECT
(8) DISTINCT <select_list>
	(1) FROM  <main_table>
	(3) <join_type> JOIN <join_table>
	(2) ON <join_condition>
	(4) WHERE <where_condition>
	(5) GROUP BY <group_by_list>
	(6) HAVING <having_condition>   ---(过滤分组后的信息，条件和where是一样的，位置不同)
	(9) ORDER BY <order_by_condition>
	(10) LIMIT <limit_number>

```


# MySQL函数
---

[MySQL ：： MySQL 8.0 参考手册 ：： 14.1 内置函数与操作符参考](https://dev.mysql.com/doc/refman/8.0/en/built-in-function-reference.html)


## 聚合函数


| 函数名称    | 描述  |
| ------- | --- |
| COUNT() | 计数  |
| SUM()   | 求和  |
| AVG()   | 平均值 |
| MAX()   | 最大值 |
| MIN()   | 最小值 |
| ...     |     |

# 数据库级别的MD5加密（选）
---
什么是MD5？
主要增强算法复杂度和不可逆性
MD5不可逆，具体的值的MD5是一样的
```sql
--数据
INSERT INTO testmd5 values(1,'zhangsan','123456'),(2,'lisi','123456')
```

```sql
---加密，插入时加密
insert into test values(4,'xiaoming',MD5('123456'))

--校验：将用户传进来的密码，进行md5加密，然后比对加密后的值

select * from testmd5 where `name`='xiaoming' and pwd=MD5('123456')

```


# 事务
---
## 什么是事务

<font color="red">要么都成功，要么都失败</font>

1、SQL执行   A 给 B 转账      A  1000   ----> 200   B 200
2、SQL执行   B 收到 A 的钱   A   800     --->  B  400

将一组SQL放在一个批次中去执行

>事务原则:  ACID 原则  原子性  一致性 隔离性  持久性


**原子性(Atomicity)**
`单个事务，为一个不可分割的最小工作单元`，整个事务中的所有操作要么全部commit成功，要么全部失败rollback，对于一个事务来说，不可能只执行其中的一部分SQL操作，这就是事务的原子性。

事务被视为一个不可分割的最小单位，它要么完全执行，要么完全不执行。

**一致性(Consistency)**
数据库总是从一个一致性的状态转换到另外一个一致性的状态。在前面的例子中， 一致性确保了，即使在执行第三、四条语句之间时系统崩潰，信用卡账户也不会损 失100块，因为事务最终没有提交，所以事务中所做的修改也不会保存到数据库中，保证数据一致性

一致性保证了事务的执行将数据库从一个一致的状态转变到另一个一致的状态。
 
 **持久性(Durability)**

一旦事务提交，则其所做的修改就会永久保存到数据库中。此时即使系统崩溃，修改的数据也不会丢失。

持久性意味着一旦事务被提交，它对数据库的修改就是永久性的，即使系统发生故障也不会丢失。

 **隔离性(Isolation)**

通常来说，一个事务所做的修改在最终提交以前，对其他事务是不可见的。在前面 的例子中，当执行完第三条语句、第四条语句还未开始时，此时有另外一个账户查询余额SQL开始运行，则其看到的信用卡账户的余额并没有被减去100元。后面我们讨论隔离级别(Isolation level)的时候，会发现为什么我们要说事务通常来说是不可见的。

隔离性是指当多个事务同时对数据库进行操作时，每个事务都是独立的，一个事务的操作不会影响到其他事务。


>隔离所导致的一些问题

**脏读**
事务 A 读到了事务 B **未提交**的数据。若 B 回滚，A 读到的就是无效数据。

**不可重复读**
同一事务内两次读同一行，结果不同。因为期间另一个事务修改并提交了该行。

**幻读**

同一事务内两次执行同一范围查询，结果集行数不同。因为**期间另一个事务插入或删除了符合条件的行。

**丢失更新**

两个事务同时读同一数据，各自修改后提交，后提交的覆盖了先提交的。  
例：库存 100，T1、T2 都读到 100，T1 改成 90，T2 改成 80，T1 的更新丢失。

**读偏斜**

事务内读取多个相关数据时，读到了不一致的快照。  
例：转账中先读 A 账户旧值，再读 B 账户新值，导致总额对不上。

**写偏斜**

两个事务读取相同的数据集，然后分别修改不同的数据，最终组合起来违反业务约束。  
经典例子：医生值班要求至少一人在岗。T1、T2 都读到两人在岗，T1 让 A 请假，T2 让 B 请假，结果无人值班。

**序列化异常**

并发执行的结果无法对应任何串行执行顺序，这是可串行化隔离要解决的根本问题。

| 隔离级别         | 效果                                                                                                        |
| ------------ | --------------------------------------------------------------------------------------------------------- |
| 读未提交（RU）     | 一个事务还没提交时，它做的变更就能被别的事务看到。（别的事务指同一时间进行的增删改查操作）                                                             |
| 读提交（RC）      | 一个事务提交（commit）之后，它做的变更才会被其他事务看到。                                                                          |
| 可重复读（RR）     | 一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。<br>当然在可重复读隔离级别下，未提交变更对其他事务也是不可见的。                                    |
| 串行（xíng）化（S） | 正如物理书上写的，串行是单线路，顾名思义在 MySQL 中同一时刻只允许单个事务执行，“写”会加“写锁”，“读”会加“读锁”。<br>当出现读写锁冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行。 |


>执行事务

MySQL默认开启事务自动提交

```sql
SET autocommit = 0 /*关闭/
SET autocommit = 1/*开启（默认的）/
```


# 索引
---
索引是数据库中用来提升查询效率的一种数据结构。它能快速定位数据，大大减少需要扫描的数据量

## 索引的分类
- 主键索引 (PRIMARY KEY
  - 唯一的标识，主键不可重复，只能有一个列作为主键
- 唯一索引 (UNIQUE KEY)
  - 避免重复的列出现，唯一索引可以重复，多个列都可以标识为唯一索引
- 常规索引 (KEY/INDEX)
  - 默认的，index。key关键字来设置
- 全文索引 (FullText)
  - 在特定的数据库引擎下才有，MyISAM
  - 快速定位数据


## 索引的使用

1、在创建表的时候给字段增加索引
2、创建完毕后，增加索引

创建索引有两种方式：

**create 索引类型 索引名 on 表（字段）

和 

**alter table 表名 add 索引类型 索引名（字段）


索引在小数据量的时候，用处不大，在大量数据时，区别十分明显

## 索引原则

- 索引不是越多越好
- 不要对经常变动数据加索引
- 小数据量的表不需要加索引
- 索引一般加载常用来查询的字段上

>索引的数据结构

Hash类型的索引

Btree：InnoDB的默认结构


## MySQL备份

为什么要备份：
- 保存重要的数据不丢失
- 数据转移
MySQL数据库备份的方式
- 直接拷贝物理文件
- 在SQLyog这种可视化工具中手动导出
- 使用命令行 mysqldump命令

**可视化导出
![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791678750253_image.png)
![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791678765153_image.png)

**命令行导出

```sql
操作        主机         用户   密码   数据库  数据库表  >物理存储位置
mysqldump -hlocalhost -uroot -p123456 test student >D:/a.sql

mysqldump -hlocalhost -uroot -p123456 test 表一 表二 表三 >D:/a.sql

mysqldump -hlocalhost -uroot -p123456 test  >D:/a.sql
```

![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791679048082_image.png)


**导入

**命令行登入后

Use 对应的数据库(如果导入表)

使用 【Source 物理地址数据库备份文件路径】

**未登录

```
mysql -u用户名 -p密码 库名<备份文件路径
```


# 规范数据库设计
---
## 为什么要设计

<font color="blue">当数据库比较复杂的时候，我们需要规范设计数据库</font>

糟糕的数据库设计：
- 数据冗余，浪费空间
- 数据插入和删除都会麻烦、异常【屏蔽使用物理外键】
- 程序的性能差

良好的数据库设计：
- 节省内存空间
- 保证数据库的完整性
- 方便我们开发系统

**软件开发中，关于数据库的设计
- 分析需求：分析业务和需要处理的数据库需求
- 概要设计：设计关系图E-R图

**设计数据库的步骤（个人博客）：
- 分析 
 - 用户表（登录注销，个人信息，写博客，创建分类）
 - 分类表（文章分类，创建者）
 - 文章表（文章的信息）
 - 友链表（友链信息）
 - 自定义表

- 标识实体（把需求落地到每个表）

- 标识实体之间的关系
 - 写博客：user->blog
 - 创建分类：user->category
 - 关注：user->user
 - 友链：links
 - 评论：user-user-blog

## 三大范式

**为什么需要数据规范化
 - 信息重复
 - 更新异常
 - 插入异常
   - 无法正常显示信息
 - 删除异常
   - 丢失有效的信息

>三大范式

**第一范式（1NF）

原子性：保证每一列不可再分

**第二范式（2NF）

原文：[数据库设计三大范式 - Ruthless - 博客园](https://www.cnblogs.com/linjiqin/archive/2012/04/01/2428695.html)

前提：满足第一范式

还要求数据库表中的每个实例或行必须被唯一标识，满足实体的属性完全依赖于主关键字

**也就是说在一个数据库表中，一个表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中。



**第三范式(3NF)

前提：满足第二范式

**第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。



# JDBC
---


1.创建空项目


2.导入JDBC驱动


3.编写测试代码


```java
package com.winskyx.lesson01;  
  
import java.sql.*;  
  
public class JDBCFirstDemo {  
    public static void main(String[] args) throws ClassNotFoundException, SQLException {  
        //加载驱动  
        Class.forName("com.mysql.jdbc.Driver");//固定写法  
  
        //用户信息和URL  
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?userUnicode=true&characterEncoding=utf8&useSSL=true";  
        String username = "root";  
        String password = "123456";  
  
  
        //连接成功，数据库对象 Connection 代表数据库  
        Connection connection = DriverManager.getConnection(url, username, password);  
  
        //执行SQL的对象   Statement执行SQL的对象  
        Statement statement = connection.createStatement();  
  
  
        //执行SQL的对象去执行SQL，可能存在结果,查看返回结果  
        String sql = "SELECT * FROM users";  
  
  
        ResultSet resultSet = statement.executeQuery(sql);//返回的结果集，结果集中封装了我们全部查询出来的结果  
  
        while (resultSet.next()){  
            //不知道类型可以为getObject  
            System.out.println("id="+resultSet.getObject("id"));;  
            System.out.println("username="+resultSet.getObject("username"));;  
            System.out.println("password="+resultSet.getObject("password"));;  
            System.out.println("================");  
        }  
  
        //释放连接  
  
        resultSet.close();  
  
        statement.close();  
  
        connection.close();  
  
  
  
    }  
  
}
```

结果

```txt
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
id=1
username=lisi
password=123456
================
id=2
username=zhangsan
password=123456
================
id=3
username=wangwu
password=123456
================
id=4
username=zhaoliu
password=123456
================
id=5
username=sunqi
password=123456
================
id=6
username=zhouba
password=123456
================
id=7
username=wujiu
password=123456
================
id=8
username=zhengshi
password=123456
================
id=9
username=qianyi
password=123456
================
id=10
username=chener
password=123456
================
id=11
username=liuyi
password=123456
================
```

步骤：

1、加载驱动

2、连接数据库DriverManager

3、获得执行sql的对象 statement

4、获得返回的结果集

5、释放连接


>DriverManager

```java
//DriverManager.registerDriver(new com.mysql.jdbc.Driver());  
Class.forName("com.mysql.jdbc.Driver");//固定写法

//connection代表数据库
Connection connection = DriverManager.getConnection(url, username, password);
//数据库设置自动提交
//事务提交
//事务回滚
connection.rollback();  
connection.commit();  
connection.setAutoCommit();
```

>URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcstudy?userUnicode=true&characterEncoding=utf8&useSSL=true";
//mysql--3306
//jdbc:mysql://主机地址:端口号/数据库名？参数1&参数2&参数3  
//oracle --1521
//jdbc:oracle:thin@localhost:1521:sid
```


>Statement执行SQL的对象  PrepareStatement执行SQL的对象

```java
String sql = "SELECT * FROM users";//编写SQL


statement.executeQuery();  //查询操作返回Resultset
statement.executeUpdate();  //执行任何SQL
statement.execute();//更新、插入、删除都用这个，返回一个受影响的行数
```


<font color='red'>JDBC中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可</font>


---
**方法executeUpdate**

用于执行 **INSERT**、**UPDATE** 或 **DELETE** 语句以及 **SQL DDL**（数据定义语言）语句，例如 **CREATE TABLE** 和 **DROP TABLE**。

INSERT、UPDATE 或 DELETE 语句的效果是修改表中零行或多行中的一列或多列。

executeUpdate 的返回值是一个整数（int），指示受影响的行数（即更新计数）。

---

**方法executeQuery**

用于产生单个结果集（ResultSet）的语句，例如:被执行最多的SELECT 语句。 

这个方法被用来执行 **SELECT** 语句，但也只能执行查询语句，执行后返回代表查询结果的ResultSet对象。


|       | executeQuery | executeUpdate                | execute    |
| ----- | ------------ | ---------------------------- | ---------- |
| SQL指令 | SELECT       | INSERT/UPDATE/DELETE/SQL DDL | ALL(任何SQL) |

>ResultSet 查询的结果集：封装了所有的查询结果

获得指定的数据类型

```java
resultSet.getObject();//在不知道列类型的情况下使用  

//如果知道列的类型就使用指定的类型  
resultSet.getString();
resultSet.getInt();  
resultSet.getFloat();  
resultSet.getDate();  
resultSet.getObject();
```

遍历，指针

```java
resultSet.beforeFirst();//移动到最前  
resultSet.afterLast();//移动到最后  
resultSet.next();//移动到下一个数据  
resultSet.previous();//移动到前一行  
resultSet.absolute(row);//移动到指定行

```

>释放资源

```java
//释放连接
resultSet.close();  
  
statement.close();  
  
connection.close();
```


>代码实现

1、提取工具类

2、编写增删改的方法，`executeUpdate`

**增
```java
public static void main(String[] args) {  
    Connection conn = null;  
    Statement st =null;  
    ResultSet rs =null;  
  
  
  
    try {  
        conn = JdbcUtils.getConnection();//获取数据库连接  
        st = conn.createStatement();//获得SQL的执行对象  
        String sql = "insert into users(id,`username`,`password`) value(100,'时间行者','RunTime')";  
  
        int i = st.executeUpdate(sql);  
  
        if(i>0){  
            System.out.println("插入成功，插入"+i+"条数据");;  
        }  
  
  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```

**删

```java
public static void main(String[] args) {  
    Connection conn = null;  
    Statement st =null;  
    ResultSet rs =null;  
  
  
  
    try {  
        conn = JdbcUtils.getConnection();//获取数据库连接  
        st = conn.createStatement();//获得SQL的执行对象  
        String sql = "DELETE FROM users WHERE id = 100";  
  
        int i = st.executeUpdate(sql);  
  
        if(i>0){  
            System.out.println("删除成功，删除"+i+"条数据");;  
        }  
  
  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```

**改

```java
public static void main(String[] args) {  
    Connection conn = null;  
    Statement st =null;  
    ResultSet rs =null;  
  
  
  
    try {  
        conn = JdbcUtils.getConnection();//获取数据库连接  
        st = conn.createStatement();//获得SQL的执行对象  
        String sql = "UPDATE users SET `username`='winskyx' WHERE id =100";  
  
        int i = st.executeUpdate(sql);  
  
        if(i>0){  
            System.out.println("修改成功，修改"+i+"条数据");;  
        }  
  
  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```

3、查询

```java
public static void main(String[] args) {  
    Connection conn =null;  
    Statement st =null;  
    ResultSet rs = null;  
  
  
    try {  
        conn =  JdbcUtils.getConnection();  
        st = conn.createStatement();  
  
        String sql = "select * from users";  
  
  
        rs=st.executeQuery(sql);  
  
  
        while (rs.next()){  
            System.out.println(rs.getInt("id"));  
            System.out.println(rs.getString("username"));  
            System.out.println(rs.getString("password"));  
            System.out.println("========");  
        }  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
  
  
}
```

**遍历 ResultSet

要遍历ResultSet对象，通常需要执行以下步骤：

1. 创建 Statement 或 PreparedStatement 对象：首先，您需要创建一个Statement或PreparedStatement对象，用于执行SQL查询。

2. 执行查询：使用Statement或PreparedStatement对象执行SQL查询，将查询结果存储在ResultSet中。

3. 遍历 ResultSet：使用循环结构（如while或for循环）和ResultSet的相关方法，逐行遍历查询结果。

4. 获取数据：通过ResultSet提供的方法，获取每一行的数据。

5. 关闭 ResultSet：在完成遍历后，及时关闭ResultSet对象以释放资源。


>SQL注入的问题

sql存在漏洞，会被攻击导致数据泄漏,SQL会被拼接

`login("'or' 1=1","'or' 1=1");`



```java
方法参数为 username ,password
String sql = "select * from users where username='"+username+"'and password='"+password+"'";
```


代入后拼接出的完整 SQL 是：

`select * from users where username=''or' 1=1'and password=''or' 1=1'`

username=''  为 0
'1=1' 为 1

在 MySQL 中，`' 1=1'` 之所以在条件判断中为 `true`，是因为 **MySQL 会把字符串隐式转换成数字**，而转换结果是 `1`，非零即真


## PreparedStatement对象

PreparedStatement可以防止SQL注入，效率更好

1、新增
```java
public static void main(String[] args) {  
    Connection conn =null;  
    PreparedStatement st =null;  
    ResultSet rs =null;  
  
  
    try {  
        conn = JdbcUtils.getConnection();  
  
  
        //区别  
        //使用?占位符代替参数  
        String sql = "insert into users(id,`username`,`password`) values(?,?,?)";  
  
        st = conn.prepareStatement(sql);//预编译SQL，先写sql，然后不执行  
  
        //手动给参数赋值  
        st.setInt(1,101);//id  
        st.setString(2,"AcidEnzyme");  
        st.setString(3,"123456");  
  
        //注意点：sql.Date  
        // util.date  java                   new Date().getTime获得时间戳  
       // st.setDate(5,new java.sql.Date(new Date().getTime()));  
  
  
        //执行  
        int i = st.executeUpdate();  
  
        if (i>0){  
            System.out.println("插入成功");  
        }  
  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```
2、删除
```java
public static void main(String[] args) {  
    Connection conn =null;  
    PreparedStatement st =null;  
    ResultSet rs =null;  
  
  
    try {  
        conn = JdbcUtils.getConnection();  
  
  
        //区别  
        //使用?占位符代替参数  
        String sql = "delete from users where id =?";  
  
        st = conn.prepareStatement(sql);//预编译SQL，先写sql，然后不执行  
  
        //手动给参数赋值  
        st.setInt(1,101);//id  
        //st.setString(2,"AcidEnzyme");       // st.setString(3,"123456");  
        //注意点：sql.Date  
        // util.date  java                   new Date().getTime获得时间戳  
        // st.setDate(5,new java.sql.Date(new Date().getTime()));  
  
  
        //执行  
        int i = st.executeUpdate();  
  
        if (i>0){  
            System.out.println("删除成功");  
        }  
  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        //资源释放  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```
3、更新
```java
public static void main(String[] args) {  
    Connection conn =null;  
    PreparedStatement st =null;  
    ResultSet rs =null;  
  
  
    try {  
        conn = JdbcUtils.getConnection();  
  
  
        //区别  
        //使用?占位符代替参数  
        String sql = "update users set `username`=? where id=?";  
  
  
  
        st = conn.prepareStatement(sql);//预编译SQL，先写sql，然后不执行  
  
        //手动给参数赋值  
        st.setInt(2,101);//id  
        st.setString(1,"Lanberdon");  
        //st.setString(3,"123456");  
  
        //注意点：sql.Date  
        // util.date  java                   new Date().getTime获得时间戳  
        // st.setDate(5,new java.sql.Date(new Date().getTime()));  
  
  
        //执行  
        int i = st.executeUpdate();  
  
        if (i>0){  
            System.out.println("修改成功");  
        }  
  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```
4、查询
```java
public static void main(String[] args) {  
    Connection conn = null;  
    PreparedStatement st =null;  
    ResultSet rs = null;  
  
  
    try {  
        conn = JdbcUtils.getConnection();  
  
        String sql ="select * from users where id =?";  
  
        st=conn.prepareStatement(sql);//预编译后会返回一个statement对象  
  
        st.setInt(1,100);  
  
        rs=st.executeQuery();  
  
        if (rs.next()){  
            System.out.println(rs.getString("username"));  
        }  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```
5.防止SQL注入
```java
public static void main(String[] args) {  
  
  
    login("winskyx","RunTime");  
  
    login("''or 1=1","''or 1=1");  
}  
  
  
//登录业务  
public  static void  login(String username,String password){  
  
  
  
  
    Connection conn =null;  
    PreparedStatement  st =null;  
    ResultSet rs = null;  
  
  
    try {  
        conn =  JdbcUtils.getConnection();  
  
  
  
        //PreparedStatement放置SQL注入，把传递进来的参数当做字符  
  
        //假设其中存在转移字符，就直接忽略  
        String sql = "select * from users where `username`=? and `password`=?";  
  
        st = conn.prepareStatement(sql);  
  
        st.setString(1,username);  
        st.setString(2,password);  
  
  
  
  
        rs=st.executeQuery();  
  
  
  
  
        while (rs.next()){  
            System.out.println(rs.getInt("id"));  
            System.out.println(rs.getString("username"));  
            System.out.println(rs.getString("password"));  
            System.out.println("========");  
        }  
  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
  
}
```



## IDEA连接MySQL数据库


**连接

![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791821830395_image.png)


![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791821861528_image.png)


![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791821896904_image.png)



若要在IDEA内的数据库操作面板修改数据，需要记得在修改完后点击绿色向上箭头提交

![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1791822067456_image.png)


## 事务

==🔴要么都成功，要么都失败==

>ACID原则

原子性：要么全部完成，要么都不完成

一致性：总数不变

**隔离性：多个进程互不干扰

持久性：一旦提交不可逆，持久到数据库了


隔离性的问题：

脏读：一个事务读取了另一个事务没有提交的事务

不可重复读：在同一个食物内，重复读取表中的数据，表数据发生了改变

虚读（幻读）：在一个事务内，读取到了别人插入的数据，导致前后读出来结果不一致

>代码实现

1、 开启事务`conn.setAutoCommit(false)

2、一组业务执行完毕，提交事务

3、可以在catch语句中显式地定义回滚语句，但默认失败就会回滚

**一个会出错回滚的案例

```java
public static void main(String[] args) {  
    Connection conn = null;  
    PreparedStatement st =null;  
    ResultSet rs =null;  
  
    try {  
        conn=JdbcUtils.getConnection();  
        //关闭数据库的自动提交，自动会开启事务  
        conn.setAutoCommit(false);//开启事务  
  
        String sql1 ="update account set money=money-100 where name='A'";  
        st=conn.prepareStatement(sql1);  
        st.executeUpdate();  
  
        int x =1/0;  
  
        String sql2 ="update account set money=money+100 where name='B'";  
        st=conn.prepareStatement(sql2);  
        st.executeUpdate();  
  
        //业务完毕，提交事务  
  
        conn.commit();  
        System.out.println("成功");  
  
    } catch (SQLException e) {  
        //如果失败，则默认回滚  
        try {  
            System.out.println("失败回滚");  
            conn.rollback ();//失败回滚  
  
        } catch (SQLException ex) {  
            ex.printStackTrace();  
        }  
    }finally {  
        JdbcUtils.release(conn,st,rs);  
    }  
}
```

## 数据库连接池

数据库连接--执行--释放   

连接--释放 十分浪费系统资源

每次创建和销毁数据库连接都会涉及到网络通信、认证等操作，这些操作会消耗大量的时间和资源。对于高并发的应用程序来说，频繁创建和销毁连接会导致性能瓶颈。

**池化技术：准备一些预先的资源，过来就连接预先准备好的

常用连接数：10个

最小连接数：10

最大连接数：100 业务最高承载上上限

等待超时：100ms


编写连接池，实现一个接口 DataSource

>开放数据源实现（拿来即用）

DBCP

C3P0

Druid:阿里巴巴

使用了这些数据库连接池之后，我们在项目开发中就不需要编写连接数据库的代码了


>DBCP

需要用到的jar包

commons-logging、commons-pool2-2.13.1-bin、commons-dbcp2-2.14.0-bin

>C3P0

需要用到的jar包

c3p0-*.jar   mchange-commons-java.jar

代码版配置
```java
dataSource = new ComboPooledDataSource();
dataSource.setDriverClass();
dataSource.setUser();
dataSource.setPassword();
dataSource.setJdbcUrl();

dataSouce.setMaxPoolSize();
```


配置文件写法
```java
dataSouce = new ComboPooledDataSource("MySQL);//
```

>结论

无论使用什么数据源，本质还是一样的，DataSource接口不会变，方法就不会变

