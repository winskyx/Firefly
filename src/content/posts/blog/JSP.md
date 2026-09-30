---
title: JSP
published: 2026-09-16
description: JSP基础语法
date: 2026-09-16
updated: 2026-09-16
tags:
  - JSP
image: ./cover.jpg
category: JavaWeb
draft: false
author: winskyx
---
# Jsp

任何语言都有自己的语法，JAVA中有，JSP作为java技术的一种应用，它拥有一些自己扩充的语法（了解即可），Java所有语法都支持！

__JSP表达式
```jsp
<%--JSP表达式  
    作用：用来将程序的输出，输出到客户端  
<%= 变量或表达式%>  
--%>  
<%= new java.util.Date() %>
```

__jsp脚本片段
```jsp
<%  
    int sum =0;  
    for (int i = 0; i < 100; i++) {  
        sum+=i;    }    out.println("<h1>Sum="+sum+"</h1>");  
%>
```

__脚本片段的再实现

```jsp
  
<%  
    int x =10;  
    out.println(x);%>  
<p>这是一个JSP文档</p>  
<% int y = 2;  
    out.println(y);%>  
<hr>  
  
<%--在代码中嵌入HTML元素--%>  
<% for (int i = 0; i < 5; i++) {  
%>  
<h1>HelloWorld<%=i %></h1>  
  
<% }  
%>
```

## JSP声明
```jsp
<%!  
static {  
    System.out.println("Loading Servlet");  
}  
private int globalVar = 0;  
public void winskyx(){  
    System.out.println("进入了方法winskyx！");  
}  
%>
```

JSP声明：会被编译到JSP生成Java的类中！其他的，就会被生成到_jspService方法中

在JSP，嵌入java代码即可


```jsp
<%%>
<%=%>
<%!%>
<%--%>
```

JSP的注释不会在客户端源代码中出现


## JSP指令
```jsp
<%@page args... %>
<%@ include file=""%>
```


## 9大内置对象
- PageContext
- Request
- Response
- Session
- Application
- config (ServletConfig)
- out
- page
- excepetion


```jsp
pageContext.setAttribute("name","winskyx");//保存的数据只在一个页面中有效  
request.setAttribute("name1","winskyx1");//保存的数据只在一次请求中有效，请求转发forward会携带这个数据  
session.setAttribute("name2","winskyx2");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器  
application.setAttribute("name3","winskyx3");//保存的数据只在服务器中有效，从打开服务器到关闭服务器
```

request：客户端向服务器发送请求，产生的数据，用户看完了就没用了，比如：新闻，用户看完失效

session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车

applicaiton：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用；比如：聊天数据

## JSP标签、JSTL标签、EL表达式

```xml
<dependency>  
  <groupId>jakarta.servlet.jsp.jstl</groupId>  
  <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>  
  <version>3.0.2</version>  
  <scope>compile</scope>  
</dependency>  
<!-- Source: https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->  
<dependency>  
  <groupId>org.apache.taglibs</groupId>  
  <artifactId>taglibs-standard-impl</artifactId>  
  <version>1.2.5</version>  
  <scope>runtime</scope>  
</dependency>
```
EL表达式：${}
- 获取数据
- 执行运算
- 获取web开发的常用对象

__JSP标签
```jsp

<jsp:forward page="jsptag2.jsp">  
  
    <jsp:param name="name" value="winskyx"/>  
  
    <jsp:param name="age" value="12"/>  
</jsp:forward>

```

__JSTL表达式

JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义许多标签，可以供我们使用，标签的功能和java代码一样！

__格式化标签

__SQL标签

__XML标签

__核心标签（掌握部分）
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```


| 标签                                                                     | 描述                                                   |
| ---------------------------------------------------------------------- | ---------------------------------------------------- |
| [<c:out>](https://www.runoob.com/jsp/jstl-core-out-tag.html)           | 用于在JSP中显示数据，就像<%= ... >                              |
| [<c:set>](https://www.runoob.com/jsp/jstl-core-set-tag.html)           | 用于保存数据                                               |
| [<c:remove>](https://www.runoob.com/jsp/jstl-core-remove-tag.html)     | 用于删除数据                                               |
| [<c:catch>](https://www.runoob.com/jsp/jstl-core-catch-tag.html)       | 用来处理产生错误的异常状况，并且将错误信息储存起来                            |
| [<c:if>](https://www.runoob.com/jsp/jstl-core-if-tag.html)             | 与我们在一般程序中用的if一样                                      |
| [<c:choose>](https://www.runoob.com/jsp/jstl-core-choose-tag.html)     | 本身只当做<c:when>和<c:otherwise>的父标签                      |
| [<c:when>](https://www.runoob.com/jsp/jstl-core-choose-tag.html)       | <c:choose>的子标签，用来判断条件是否成立                            |
| [<c:otherwise>](https://www.runoob.com/jsp/jstl-core-choose-tag.html)  | <c:choose>的子标签，接在<c:when>标签后，当<c:when>标签判断为false时被执行 |
| [<c:import>](https://www.runoob.com/jsp/jstl-core-import-tag.html)     | 检索一个绝对或相对 URL，然后将其内容暴露给页面                            |
| [<c:forEach>](https://www.runoob.com/jsp/jstl-core-foreach-tag.html)   | 基础迭代标签，接受多种集合类型                                      |
| [<c:forTokens>](https://www.runoob.com/jsp/jstl-core-foreach-tag.html) | 根据指定的分隔符来分隔内容并迭代输出                                   |
| [<c:param>](https://www.runoob.com/jsp/jstl-core-param-tag.html)       | 用来给包含或重定向的页面传递参数                                     |
| [<c:redirect>](https://www.runoob.com/jsp/jstl-core-redirect-tag.html) | 重定向至一个新的URL.                                         |
| [<c:url>](https://www.runoob.com/jsp/jstl-core-url-tag.html)           | 使用可选的查询参数来创造一个URL                                    |
|                                                                        |                                                      |



JSTL标签库使用步骤
- 引入对应的taglib
- 使用其中的方法
