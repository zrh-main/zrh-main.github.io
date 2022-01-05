---
title: Java Servlet使用
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

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
