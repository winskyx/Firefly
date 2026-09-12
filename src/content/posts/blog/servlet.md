---
title: Servlet
published: 2026-09-11
description: 文章的描述
date: 2026-09-11
updated: 2026-09-12
tags:
  - JavaWeb
image: ./cover.jpg
category: JavaWeb
draft: false
author: winskyx
---
# Servlet
---
## Servlet简介
- Servlet就是Sun公司开发动态Web的一门技术
- Sun公司在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤:
  - 编写一个类，实现Servlet接口
  - 把开发好的Java类部署到Web服务器中
  把实现了Servlet接口的Java程序叫做Servlet
# HelloServlet

1.构建一个普通的Maven项目

2.关于Maven父子工程的理解
   父项目中会有```
   ```xml
<modules>  
      <module>Servlet-01</module>  
</modules>
   ```
   子项目会有`
   ```xml
   <parent>  
    <groupId>org.example</groupId>  
    <artifactId>JavaWeb-Servlet</artifactId>  
    <version>1.0-SNAPSHOT</version>  
</parent>
   ```
父项目中的Java子项目可以直接使用
```
son extend father
```
Maven环境优化
 - 修改web.xml为最新的
 - 将Maven的结构搭建完整
编写一个Servlet程序
 - 编写一个普通类
 - 实现Servlet接口，这里我们直接继承HttpServlet
```java
public class HelloServlet extends HttpServlet {  
    //  由于GET和POST只是请求实现的不同的方式，可以相互调用
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        super.doGet(req, resp);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        super.doPost(req, resp);  
    }  
}
```
编写Servlet的映射
   为什么需要映射：我们写的是JAVA程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务中注册我们写的Servlet，还需要给他一个浏览器能访问的路径
   
   个人理解：服务器启动后，访问/hello后会运行HelloServlet，


web.xml配置
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app version="4.0"  
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee  
          http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"         metadata-complete="true">  
     <servlet>  
        <servlet-name>hello</servlet-name>  
        <servlet-class>com.winskyx.servlet.HelloServlet</servlet-class>  
    </servlet>    
    <servlet-mapping>       
     <servlet-name>hello</servlet-name>  
        <url-pattern>/hello</url-pattern>  
    </servlet-mapping></web-app>
```
配置Tomcat
   注意：配置项目发布的路径即可

启动测试




### 注意事项

如果
```xml
<servlet-name>hello</servlet-name>  
        <url-pattern>hello</url-pattern>  
    </servlet-mapping></web-app>
```
中
```xml
 <url-pattern>hello</url-pattern>  
```
没有加斜杠"   /    "会出现报错
```txt
java.lang.IllegalArgumentException: servlet映射中的<url pattern>[hello]无效
```


## Mapping问题

1.一个servlet可以请求一个映射路径
```xml
<servlet-mapping>  
    <servlet-name>hello</servlet-name>  
    <url-pattern>/hello</url-pattern>  
</servlet-mapping>
```
2.一个servlet请求可以请求多个映射路径
```xml
<servlet-mapping>  
    <servlet-name>hello</servlet-name>  
    <url-pattern>/hello</url-pattern>  
</servlet-mapping>
<servlet-mapping>  
    <servlet-name>hello</servlet-name>  
    <url-pattern>/hello2</url-pattern>  
</servlet-mapping>
```
3.一个servlet请求可以请求通用的映射路径
```xml
<servlet-mapping>  
    <servlet-name>hello</servlet-name>  
    <url-pattern>/hello/*</url-pattern>  
</servlet-mapping>
```

__默认请求路径__
```xml
<servlet-mapping>  
    <servlet-name>hello</servlet-name>  
    <url-pattern>/*</url-pattern>  
</servlet-mapping>
```
*可以自定义后缀实现请求*   *前面不能加映射的路径
```xml
<servlet-mapping>  
    <servlet-name>hello</servlet-name>  
    <url-pattern>*.winskyx</url-pattern>  
</servlet-mapping>
```


## ServletContext

### 共享数据
web容器启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；
- 共享数据
   我在这个Servlet中保存的数据，可以在另外一个Servlet中拿到

/hello映射 创建一个值 username = wiskyx   __在Context中值名称为name 而非 变量 username__
```java
public class HelloServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        System.out.println("Hello-Servlet-02");  
  
        ServletContext servletContext = this.getServletContext();  
  
        String username = "winskyx";  
        servletContext.setAttribute("name",username);//将一个数据保存在了ServletContext中,名字为name，值为username  
  
    }  
}
```

/GetServlet映射 获取值并输出

```java
public class GetServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ServletContext context = this.getServletContext();  
  
  
        String username = (String) context.getAttribute("name");  
		----------
        resp.getWriter().print("name:"+username);  
        ----------
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        super.doPost(req, resp);  
    }  
}
```
```xml
<servlet>  
  <servlet-name>hello</servlet-name>  
  <servlet-class>com.winskyx.servlet.HelloServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
  <servlet-name>hello</servlet-name>  
  <url-pattern>/hello</url-pattern>  
</servlet-mapping>  
  
  
  
<servlet>  
  <servlet-name>GetServlet</servlet-name>  
  <servlet-class>com.winskyx.servlet.GetServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
  <servlet-name>GetServlet</servlet-name>  
  <url-pattern>/getServlet</url-pattern>  
</servlet-mapping>
```

  先访问/hello，创建一个值为winskyx名称为name的Context属性，再访问/getServlet获取Context的值
  输出结果:
  __name:winskyx__


### 获取初始化参数

```xml
<!-- 配置一些web应用初始化参数 -->  
  <context-param>  
    <param-name>url</param-name>  
    <param-value>jdbc:mysql:localhost:3306/mybatis</param-value>  
  </context-param>
```

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
    ServletContext context = this.getServletContext();  
  
    String url = context.getInitParameter("url");  
  
    resp.getWriter().print(url);  
  
  
}
```

### 请求转发

#### 转发与重定向

__重定向__

```mermaid
sequenceDiagram
    autonumber
    participant B as 浏览器
    participant S as 服务器
    participant T as 新地址/服务器B

    B->>S: 请求 /old
    S-->>B: 301/302 Location: /new
    Note over B: 浏览器收到重定向，地址栏变为 /new
    B->>T: 重新发起请求 /new
    T-->>B: 返回 /new 内容
    Note over B,T: 两次请求，客户端参与
```
__转发__
```mermaid
sequenceDiagram
    autonumber
    participant B as 浏览器
    participant S as 服务器
    participant R as 目标资源/组件

    B->>S: 请求 /old
    S->>R: 服务器内部转发 forward 到 /new
    R-->>S: 返回 /new 处理结果
    S-->>B: 返回结果
    Note over B,S: 浏览器地址栏保持 /old，只有一次请求
```

### 读取资源文件

Properties
 - 在java目录下新建properties
 - 在resource目录下新建properties
发现：都被打包到了同一个路径下：classes，我们俗称这个路径为classpath

```properties
username=winskyx  
password=123456
```

```java
public class ServletDemo05 extends HttpServlet {  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");  
  
        Properties properties = new Properties();  
        properties.load(is);  
        String username = properties.getProperty("username");  
        String password = properties.getProperty("password");  
  
        resp.getWriter().print(username+":"+password);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        super.doPost(req, resp);  
    }  
}me+":"+password);  
}
```

访问测试即可