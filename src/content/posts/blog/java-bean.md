---
title: JavaBean
published: 2026-09-17
description:
date: 2026-09-17
updated: 2026-09-17
tags:
  - JavaWeb
image: ./cover.jpg
category: JavaWeb
draft: false
author: winskyx
---
# JavaBean
---
实体类

JavaBean有特定的写法：
- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法
一般用来和数据库的字段做映射 ORM;

ORM:对象关系映射
- 表--->类
- 字段-->属性
- 行记录--->对象


__people表

| id  | name       | age | address |
| --- | ---------- | --- | ------- |
| 1   | winskyx    | 3   | 梧州      |
| 2   | lanberdon  | 18  | 南宁      |
| 3   | acidenzyme | 100 | 广州      |


```java
Class People{
	private int id;
	private String name;
	private int id;
	private String address;
}
Class A{
	new People(1,"winskyx",3,"梧州");
	new People(2,"lanberdon",3,"梧州");
	new People(3,"acidenzyme",3,"梧州");
}
```



