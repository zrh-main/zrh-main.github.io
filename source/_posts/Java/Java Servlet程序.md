---
title: Java Servlet
date: 2022-01-03 19:49:57
tags:
  - Java重点
categories:
  Java
---


# Servlet

## 起源
  HTML只能用来保存静态内容，而静态页面很难满足实际应用的需要，因此，动态页面的概念被引入
  所谓动态页面，指的是能够根据不同时间、不同用户而显示不同内容的页面，例如常见的论坛、留言板、电子商务网站等都是通过动态页面来实现的
  那么如何才能生成动态页面呢？其中一种方法是采用公共网关接口（Common Gateway Interface，CGI）。`CGI`是一种用Perl脚本编写的程序，可以用来生成动态页面，而另一种方式则是采用`Servlet`技术。
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

## jsp和jspx的区别
  jsp的格式，其中包含“<%”声明符，jsp文件通常在服务器端处理后呈现为html代码，尽管jsp通常的目的是处理web页面，但是jsp的代码呈现却不是我们希望的html或xml格式，代码非常混乱，这也是为什么出现jspx
  jspx完全符号xml语法规范，这种规范化会带来很多的好处，我们编码会方便很多，如xml形式方便代码格式化，便于编辑呈现
  jspx：以xml语法来书写jsp的文件，自定义的映射类型, jspx = jsp + XML

## JSP与Servlet有何异同

JSP与Servlet的相同点为：JSP可以被看作一个特殊的Servlet，它只不过是对Servlet的扩展，只要是JSP可以完成的工作，使用Servlet都可以完成，例如生成动态页面。由于JSP页面最终要被转换成Servlet来运行，因此处理请求实际上是编译后的Servlet

## idea和tomcat的相关配置
1.idea会为每一个tomcat部署的项目单独创建一份配置文件