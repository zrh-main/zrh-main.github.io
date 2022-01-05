---
title: Java Servlet使用
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---



## jsp和jspx的区别
  jsp的格式，其中包含“<%”声明符，jsp文件通常在服务器端处理后呈现为html代码，尽管jsp通常的目的是处理web页面，但是jsp的代码呈现却不是我们希望的html或xml格式，代码非常混乱，这也是为什么出现jspx
  jspx完全符号xml语法规范，这种规范化会带来很多的好处，我们编码会方便很多，如xml形式方便代码格式化，便于编辑呈现
  jspx：以xml语法来书写jsp的文件，自定义的映射类型, jspx = jsp + XML

## JSP与Servlet有何异同

JSP与Servlet的相同点为：JSP可以被看作一个特殊的Servlet，它只不过是对Servlet的扩展，只要是JSP可以完成的工作，使用Servlet都可以完成，例如生成动态页面。由于JSP页面最终要被转换成Servlet来运行，因此处理请求实际上是编译后的Servlet