---
title: Filter
published: 2026-09-18
description: 文章的描述
date: 2026-09-18
updated: 2026-09-18
tags:
  - JavaWeb
image: ./cover.jpg
category: 教程
draft: false
author: winskyx
---
# Filter
---

Filter:过滤器，用来过滤网站的数据；
- 处理中文乱码
- 登录验证...

![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1789709960320_image.png)


Filter开发步骤

1.导包

2.编写过滤器

实现Filter接口，重写对应的方法即可
```java
package com.winskyx.filter;  
  
import jakarta.servlet.*;  
  
import java.io.IOException;  
  
public class CharacterEncodingFilter implements Filter {  
    //初始化  
    @Override  
    public void init(FilterConfig filterConfig) throws ServletException {  
        System.out.println("CharacterEncodingFilter初始化");  
    }  
  
    //Chain : 链  
    //1.过滤器中的所有代码，在过滤特定请求的时候都会后自行，  
//    2.必须要让过滤器继续同行  
    @Override  
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {  
        request.setCharacterEncoding("utf-8");  
        response.setCharacterEncoding("utf-8");  
        response.setContentType("text/html;charset=UTF-8");  
        System.out.println("CharacterEncodingFilter执行前");  
        chain.doFilter(request,response);//让我们的请求继续走，如果不写，程序到这里就会拦截停止  
        System.out.println("CharacterEncodingFilter执行后");  
    }  
    //销毁:Web服务器关闭的时候会销毁  
    @Override  
    public void destroy() {  
        System.out.println("CharacterEncodingFilter销毁");  
    }  
}
```

在Web.xml中配置Filter

```xml
<filter>  
<filter-name>CharacterEncodingFilter</filter-name>  
<filter-class>com.winskyx.filter.CharacterEncodingFilter</filter-class>  
</filter>    
<filter-mapping> 
<filter-name>CharacterEncodingFilter</filter-name>  
<!--        只要是/showServlet的任何请求，都会经过这个过滤器-->  
<url-pattern>/showServlet</url-pattern>  
</filter-mapping>
```


# 监听器

1.实现监听器接口

```java
package com.winskyx.listener;  
  
import com.mysql.cj.Session;  
import jakarta.servlet.ServletContext;  
import jakarta.servlet.http.HttpSessionEvent;  
import jakarta.servlet.http.HttpSessionListener;  
  
import java.net.http.WebSocket;  
  
//统计网站在线人数:统计session  
public class OnlineCountListener implements HttpSessionListener {  
    //创建session监听：看你的一举一动  
    //一旦创建Session就会触发一次这个事件  
  
  
    @Override  
    public void sessionCreated(HttpSessionEvent se) {  
        ServletContext servletContext = se.getSession().getServletContext();  
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");  
  
        System.out.println(se.getSession().getId());  
  
        if (onlineCount==null){  
            onlineCount = new Integer(1);  
        }else {  
            int count = onlineCount.intValue();  
  
            onlineCount = new Integer(count+1);  
        }  
        servletContext.setAttribute("OnlineCount",onlineCount);  
    }  
    //销毁session监听  
    //一旦销毁Session就会触发一次这个事件  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        ServletContext servletContext = se.getSession().getServletContext();  
  
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");  
  
        if (onlineCount==null){  
            onlineCount = new Integer(1);  
        }else {  
            int count = onlineCount.intValue();  
  
            onlineCount = new Integer(count-1);  
        }  
        servletContext.setAttribute("OnlineCount",onlineCount);  
    }  
  
    /*  
    * session销毁  
    * 1.手动销毁  se.getSession().invalidate();    * 2.自动销毁  web.xml配置 session-config    *    * */  
}
```

2、注册监听器
```jsp
<listener>  
    <listener-class>com.winskyx.listener.OnlineCountListener</listener-class>  
</listener>
```

# 过滤、监听器常见应用
---

用户登录之后才能进入主页!用户主注销后就不能进入主页了

1.用户登录之后，向Session中放入用户的数据

2.进入主页的时候要判断用户是否已经登录(过滤器)
```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {  
    //ServletRequest  HttpServletRequest  
  
  
    HttpServletRequest req = (HttpServletRequest) request;  
    HttpServletResponse resp = (HttpServletResponse) response;  
  
    if (req.getSession().getAttribute("USER_SESSION") == null) {  
        System.out.println("过滤器USER_SESSION是否为空判断");  
        req.getSession().setAttribute("USER_SESSION",req.getSession().getId());  
         resp.sendRedirect(req.getContextPath()+"/login.jsp");  
  
  
    }else {  
  
        resp.sendRedirect(req.getContextPath()+"/login.jsp");  
    }  
  
    chain.doFilter(request,response);  
}
```

