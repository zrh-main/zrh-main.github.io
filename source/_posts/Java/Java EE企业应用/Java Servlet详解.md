---
title: Java Servlet介绍
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---


# Servlet

## 静态资源与动态资源
  - 静态资源
    前端的固定页面，包含HTML、CSS、JS、图片等，不需要查数据库也不需要请求处理，直接就能够显示，想修改内容则必须修改页面
  - 动态资源
    需要程序处理或者从数据库中读数据，能够根据不同的条件在页面显示不同的数据，内容更新不需要修改页面
    `servlet` `jsp` `asp` `html(Ajax)`

## 概述
  - Servlet 是运行在 Web 服务器或应用服务器上的程序;全称Java Servlet;用Java编写的`服务器端程序`
  - 狭义的Servlet是指Java语言实现的一个接口
  - 广义的Servlet是指任何实现了这个Servlet接口的类
  - 位置: `tomcat/lib`存在servlet包
## 优点
  - 性能更好
  - Servlet 在 Web 服务器的地址空间内执行。没有必要再创建一个单独的进程来处理每个客户端请求
  - Servlet 是独立于平台的，因为它们是用 Java 编写的。
  - 服务器上的 Java 安全管理器执行了一系列限制，以保护服务器计算机上的资源。因此，Servlet 是可信的。
  - Java 类库的全部功能对 Servlet 来说都是可用的。它可以通过 sockets 和 RMI 机制与 applets、数据库或其他软件进行交互

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

## 注意
  - **<font color='red' size='4.8'>Servlet类型的类都是单例模式,每个类都只能有一个实例,存在线程安全问题</font>**
  - **<font color='red' size='4.8'>避免在Servlet中定义成员变量</font>**  
## 工作原理
Servlet容器(tomcat)将Servlet类载入内存，使用反射生成Servlet实例并调用它具体的方法
![](tomcat工作机制.gif)

## Servlet 的生命周期
  - `void init()`：初始化方法，在第一次访问时执行，只执行1次
    - 可以来执行初始化工作。调用这个方法时，Servlet容器会传入一个ServletConfig对象从而对Servlet对象进行初始化
  - `void service(ServletRequest sq, ServletResponse sp)`：提供服务的方法，每次访问时都会执行，执行多次
    - 用来处理客户端的请求
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
    **<font color='red'>`servlet`与`servlet-mapping`中的`servlet-name`必须一致</font>**
  - 方式二
    servlet3.0后，支持类注解配置URL映射:`@webServlet("/访问路径")`
    ![](@WebServlet配置属性.png)
    使用方式:
      `@webServlet(urlPatterns={访问路径1,访问路径2})`
      `@webServlet(value={访问路径1,访问路径2})`value可以简写可以省略


## Servlet体系结构及使用
  - `Servlet`接口
    - 某个类实现了`Servlet`接口之后，能够被服务器识别并使用;
    - 常用的方法是`service`,但是需要重写所有方法
  - `GenericServlet`抽象类实现了`Servlet`接口
    - 这个类把除了servier方法之外的方法都做了空实现;
    - 可以继承`GenericServlet`抽象类;只需要重写`service`方法就够了，当用别的方法的时，再重写别的方法
  - `HTTPServlet`抽象类   
    - HttpServlet类继承自`GenericServlet`抽象类,并且对http协议的处理做了一些封装,更方便使用
    - <font color='red'>推荐使用继承该类的方式进行Servlet的开发</font>

## HttpServlet使用

### 方法
  - `void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {//post请求进入此方法}`
  - `void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {//get请求进入此方法}`

### Servlet资源路径的相关配置
  1. 一个Servlet可以定义多个访问路径： 			
  `@webServlet({"/demo4","/demo44","/demo444"})`
  2. 路径的定义规则：
    - /xxx ：路径匹配
    - /xxx/xxx  ：多层路径，目录结构
    - *.do   XXX.do就可以访问
