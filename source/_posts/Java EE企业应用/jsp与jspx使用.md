---
title: Java Servlet使用
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

## 概述
  - JSP全称Java Server Pages，是一种动态网页开发技术
  - JSP是一种加强版的Java servlet，主要用于实现Java web应用程序的用户界面部分

## 原理
  1. Web 服务器识别出这是对 JSP 网页的请求，并且将该请求传递给 JSP 引擎。通过使用 URL或者 .jsp 文件来完成
  2. JSP 引擎从磁盘中载入 JSP 文件，然后将它们转化为 Servlet
  3. JSP 引擎将 Servlet 编译成可执行类，并且将原始请求传递给 Servlet 引擎
  4. Web 服务器的某组件将会调用 Servlet 引擎，然后载入并执行 Servlet 类
  5. Web 服务器以静态 HTML 网页的形式将 HTTP response 返回到您的浏览器中

## JSP 生命周期
  - 编译阶段：servlet容器编译servlet源文件，生成servlet类
  - 初始化阶段：加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法
  - 执行阶段：调用与JSP对应的servlet实例的服务方法
  - 销毁阶段：调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例

## jsp中文编码问题
``` jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
```
## jsp表达式
``` jsp
<%= java表达式 %>
```
## jsp语法
``` jsp
<% java代码片段 %>
```
## jsp注释
``` jsp
<%-- 该部分注释在网页中不会被显示--%> 
```
## jsp指令
  - 格式:`<%@ 指令名称 属性名=值 属性名=值 ... %>`
  - 分类
    1. page：  配置jsp页面
      * contentType：等同于repsonse.setContentType()
        1. 设置响应体的mime类型及字符集
        2. 设置当前jsp页面的编码
      * import：      导包
      * errorPage:    当前页面发生异常后，会自动跳转的指定的错误页面
      * isErrorPage： 标识当前的页面是否是错误页面。
      * true：        使用内置对象execption
      * false：       默认值。不使用execption对象。
    2. include ： 页面包含的。导入页面的资源文件。
      - 例:`<%@ include file="top.jsp" %>`
    3. taglib： 导入资源
      - 例:`<%@ taglib prefix="c"  uri="java.sun.com/jsp/jstl/core" %>`
        - `prefix`：前缀


## JSP隐含对象
`request`       HttpServletRequest类的实例
`response`	    HttpServletResponse类的实例
`out`           PrintWriter类的实例，用于把结果输出至网页上
`session`       HttpSession类的实例
`application`   ServletContext类的实例，与应用上下文有关
`config`        ServletConfig类的实例
`pageContext`	  PageContext类的实例，提供对JSP页面所有对象以及命名空间的访问
`page`	        类似于Java类中的this关键字
`exception`	    exception 类的对象，代表发生错误的 JSP 页面中对应的异常对象

## EL表达式
达式语句
### 作用		
  替换和简化jsp的java代码编写（可以从域中取数据）
### 语法
  - ${表达式}
### 注意事项
  - jsp默认支持el表达式。如果要忽略el表达式
  - 设置jsp中page指令，  isELIgnored=”true”，禁用页面中el表达式
  - \${表达式}：忽略当前的这个el表达式
### 使用
  - 运算
    1. 算术运算符  +   -  *  /(div)    %(mod)  
    2. 比较运算符 ：  >    <   >=   <=    ==   !=
    3. 逻辑运算符：  &&(and)    ||(or)   !(not)
    4. 空运算符：empty
    * 功能：判断字符串，集合，数组对是否为空或长度为0   
  - 获取值
    - 语法：
      - ${域名称.键名}：从指定域中获取键值
      - 1.pageScope			      --> pageContext    
      - 2.requestScope		    -->request          
      - 3.sessionScope		    -->session           
      - 4.applicationScope		-->application(ServletContext)
    - 说明：
    如果省略掉了域名称，会默认从最小的域中依此去找<font color='red'>！！！el表达式只能从域中取数据</font>
  - 获取对象，list集合，Map集合的值
      1. 对象
        - 对象名.属性名
      2. List集合
        - 集合名.get(索引)   集合名[索引]
      3. Map集合
        - ${集合名.key名称}
        - ${集合名[“key名称”]}
  - 隐式对象。
    * el表达式中有11个隐式对象。
    * 获取jsp其它的八大内置对象
    <font color='red'>在jsp页面中动态获取出虚拟目录${pageContext.request.ContextPath}</font>

## jstl标签库
### 概述
  - apache公司提供的产品，免费的。作用：el表达式没有循环和分支，但是jstl标签	库有
### 作用
  - 简化jsp页面的代码
### 使用步骤：
  1. 导入jar包
  2. 使用tablib引入标签库
  3. 使用标签
  4. 常用的jstl标签
    1. if：相当于java中的if语句
      - 属性：
        - test：必要属性。接收布尔值。
        * 如果表达式为true，执行标签体内容，否则，不执行。
    2. choose：相当于java中的switch语句
      - when：相当于case
      - otherwise：  default
    3. foreach
      - 普通for:
        - begin="开始" 
        - end="结束"  
        - step="步长"
      - 增强for：
        - var=""  		增强for中的变量名
        - items=""  	增强for中的集合名
      - varStatus=""  取出个数和索引。


## jsp和jspx的区别
  jsp的格式，其中包含“<%”声明符，jsp文件通常在服务器端处理后呈现为html代码，尽管jsp通常的目的是处理web页面，但是jsp的代码呈现却不是我们希望的html或xml格式，代码非常混乱，这也是为什么出现jspx
  jspx完全符号xml语法规范，这种规范化会带来很多的好处，我们编码会方便很多，如xml形式方便代码格式化，便于编辑呈现
  jspx：以xml语法来书写jsp的文件，自定义的映射类型, jspx = jsp + XML

## JSP与Servlet有何异同

JSP与Servlet的相同点为：JSP可以被看作一个特殊的Servlet，它只不过是对Servlet的扩展，只要是JSP可以完成的工作，使用Servlet都可以完成，例如生成动态页面。由于JSP页面最终要被转换成Servlet来运行，因此处理请求实际上是编译后的Servlet