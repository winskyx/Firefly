---
title: Tomcat简易文件结构和网站发布
published: 2026-09-09
description: 对Tomcat的配置和简易网站发布
date: 2026-09-09
updated: 2026-09-09
tags:
  - JavaWeb
image: https://tu.winskyx.xyz/file/1788971257683_tomcat.svg
category: JavaWeb
draft: false
author: winskyx
---
# Tomcat


文件结构

	bin        启动、关闭的脚本文件
	conf       配置
	 lib       依赖的jar包
	 logs      日志
	 webapps   存放网站的文件



bin下的startup.bat文件启动tomcat

 通过 http://localhost:8080 访问

![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1788967630029_image.png)


__开启__ ----startup.bat
__关闭__ ----shutdown.bat 或者关闭CMD窗口





![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1788968222360_image.png)

可以配置启动的端口号
- tomcat的默认端口号为:8080
- mysql:3306
- http:80
- https:443

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

可以配置的主机明层
- 默认的主机名为: localhost->127.0.0.1
- 默认的网站应用存放的位置为: webapps

```xml
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
```

网站是如何访问的
  1.输入一个域名；回车
  2.检查本机的C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射
   1.有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序
   2.没有：去DNS服务器找，找到的话就返回，找不到就返回找不到
  3.



![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1788969485574_image.png)4.可以配置一下环境变量(可选)

## 发布一个网站

1. 在webapps文件夹下创建一个winskyx文件夹
2. 复制WEB-INF文件夹及其内容
3. 在WEB-INF同级目录下（即winskyx文件夹）添加一个index.html文件
4. 编码一个简易的html文件(目前为菜鸟教程复制)
  ```html
  <html>
   <head>
   <!--添加编码方式，否则浏览器乱码 -->
    <meta charset="UTF-8">
     <title>第一个Html文档</title>
   </head>
   <body>
     欢迎访问<a href="http://www.runoob.com/">菜鸟教程</a>！
   </body>
</html>
  ```
5. 运行starup.bat文件夹
6. 浏览器输入localhost:8080/winskyx
 