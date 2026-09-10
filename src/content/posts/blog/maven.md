---
title: Maven
published: 2026-09-10
description: JavaWeb阶段学习
date: 2026-09-10
updated: 2026-09-10
tags:
  - JavaWeb
image: ./cover.jpg
category: JavaWeb
draft: false
author: winskyx
---
# Maven
___
__我为什么要学习这个技术?__
  1.在JavaWeb开发中，需要使用大量的jar包，我们手动取导入；
  2.如何能够让一个东西帮我导入和配置这个jar包
   由此，Maven诞生了

## Maven项目架构管理工具

我们目前用来就是方便导入jar包的！

maven的核心思想：约定大于配置

- 有约束，不要去违反
Maven会规定好你该如何去编写我们的Java代码，必须要按照这个规范来；

## 下载安装Maven

官网[Download Apache Maven – Maven](https://maven.apache.org/download.cgi)


![{737FFA94-6887-4F56-B053-10339597F75D}.png](https://tu.winskyx.xyz/file/blog/wenzhang/1789022972888__737FFA94-6887-4F56-B053-10339597F75D_.png)
下载完成后，解压即可；

__配置环境变量__

在我们的系统环境变量中
配置如下配置
* M2_HOME   maven目录下的bin目录
* MAVEN_HOME   maven目录
* 在系统的path中配置%MAVEN_HOME%\bin

测试Maven是否配置成功
![image.png](https://tu.winskyx.xyz/file/blog/wenzhang/1789024671121_image.png)


## 镜像

*镜像：mirror 
 - 作用：加速我们的下载

```xml
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```
## 本地仓库

在本地仓库，远程仓库；

建立一个本地仓库:localRespository
```xml
  <localRepository>F:\Enviroment\maven-resp</localRepository>
```


## POM文件

pom.xml是Maven的核心配置文件

```xml
//Maven版本和头文件
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  //这里是在
  <groupId>org.example</groupId>  
  <artifactId>HelloWorld-Maven2</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  //Package：项目的打包方式  jar：java应用   war：javaweb应用
  <packaging>war</packaging>  
  <modelVersion>4.0.0</modelVersion>  
  <name>HelloWorld-Maven2 Maven Webapp</name>  
  <url>http://maven.apache.org</url>  
  
  //项目依赖
  //Maven的高级之处在于，如果你导入一个jar包，它会帮你导入该jar包依赖的其他jar包
  <dependencies>    <dependency>      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>3.8.1</version>  
      <scope>test</scope>  
    </dependency>  </dependencies>  <build>    <finalName>HelloWorld-Maven2</finalName>  
  </build></project>

```

Maven由于它的约定大于配置，之后可能会遇到我们写的配置文件，无法被导出或生效的问题

可能的解决方案

```xml
<!-- 在build中配置resource，来防止我们资源导出失败的问题-->
<build>
    .......
      <resources>
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <exclude>**/*.properties</exclude>
                <exclude>**/*.xml</exclude>
             </excludes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
    ......
</build>
```