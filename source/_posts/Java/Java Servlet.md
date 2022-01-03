---
title: Java Servlet
date: 2022-1-03 16:09:58
tags:
  - Java重点
categories:
  Java
---

# Servlet

## 概述
  - Servlet（Server Applet），全称Java Servlet ; 用Java编写的`服务器端程序`
  - 狭义的Servlet是指Java语言实现的一个接口
  - 广义的Servlet是指任何实现了这个Servlet接口的类

## 运行环境
  - 运行于Servlet容器中
  - Servlet容器:`支持Java的应用服务器`
  - 例:`tomcat`

## 作用
  - 主要功能在于交互式地浏览和修改数据，生成动态Web内容
  - 实现上讲，Servlet可以响应任何类型的请求，但大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器

## 工作模式
  1. 客户端发送请求至服务器
  2. 服务器启动并调用Servlet，Servlet根据客户端请求生成响应内容并将其传给服务器
  3. 服务器将响应返回客户端

## Servlet 的使用方法
  - 下载servlet jar包;导入项目中;
  1. 直接使用: 直接实现Servlet接口
  2. 间接使用: 在扩展实现这个接口的类时，间接实现它

## 注意
  - **<font color='red' size='4.8'>Servlet类型的类都是单例模式,每个类都只能有一个实例,存在线程安全问题</font>**
    
## 工作原理
Servlet容器(tomcat)将Servlet类载入内存，使用反射生成Servlet实例并调用它具体的方法
![](tomcat工作机制.gif)

## Servlet 的生命周期
  - `void init()`：初始化方法，在第一次访问时执行，只执行1次
    - 可以来执行初始化工作。调用这个方法时，Servlet容器会传入一个ServletConfig对象从而对Servlet对象进行初始化
  - `void service(ServletRequest sq, ServletResponse sp)`：提供服务的方法，每次访问时都会执行，执行多次
  - `void destory()`：销毁的方法。在Servlet容器正常关闭之后执行1次
    - 常用来清除内存占用,释放资源

## Servlet 映射
  - 方式一
    `web.xml`中配置
    ``` xml
      <!-- Servlet -->
      <servlet>
        <!-- Servlet服务名 -->
        <servlet-name>/demo1</servlet-name>
        <!-- Servlet类的全限定名 -->
        <servlet-class>cn.dy.servlet.ServletDemo1</servlet-class>
      </servlet>
      <!-- Servlet映射 -->
      <servlet-mapping>
        <!-- Servlet服务名 -->
        <servlet-name>/demo1</servlet-name>
        <!-- Servlet服务地址 -->
        <url-pattern>/demo222</url-pattern>
      </servlet-mapping>
    ```
    `servlet`与`servlet-mapping`中的`servlet-name`必须一致
  - 方式二
    servlet3.0后，支持类注解配置:`@webServlet("/访问路径")`