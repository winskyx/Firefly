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