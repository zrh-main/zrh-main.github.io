---
title: Request与Response详解
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

## Request(请求)与Response(响应)
  - request和response对象由服务器创建 ; 程序员使用
  - request对象是用来获取请求信息 ; response对象用来设置响应信息

## Request

### Request继承体系结构
`ServletRequest`接口
`HttpServletRequest`接口继承自`ServletRequest`接口;在`ServletRequest`的基础上新增一些额外的方法

### HttpServletRequest方法

#### 获取请求行的数据
  - `String getMethod()`      获取请求方式
  - `String getContextPath()` 获取资源目录地址(上下文地址)
  - `String getServletPath()` 获取servlet路径
  - `String getQueryString()` 获取get请求的请求参数
  - `String getRequestURI()`  获取请求URI (统一资源标识符)
  - `String getRequestURL()`  获取请求URL (统一资源定位符)
  - `String getProtocol()`    获取协议和版本
  - `String getRemoteAddr()`  获取客户端的IP地址

#### 获取请求头的数据
  - `String getHeader(String name)`         通过请求头的键获取请求头的值
  - `Enumeration<String> getHeaderNames()`  获取所有的请求头名称

#### 获取请求参数的通用方式：
  - `getParameter(String name)`               通过参数名获取参数的值  
  - `getParameterValues(String name)`         通过参数名获取参数值的数组
  - `Enumeration<String> getParameterNames()` 获取所有的请求的参数名
  - `Map<String,String[]> getParamenterMap()` 获取所有参数的map集合

#### 获取请求体的数据
  1. 获取流对象
    `BufferedReader getReader()`          获取字符输入流
    `ServletInputStream getInputStream()` 获取字节输入流
  2. 从流中拿数据

#### 设置编码格式
  - `void setCharacterEncoding("utf-8")`   覆盖请求体中使用的字符编码 <font color='red'>必须在读取请求参数或使用getReader()读取输入之前调用</font>  

#### 获取请求转发对象
  - `request.getRequestDispatcher()`              获取请求转发对象(请求分配器对象)

#### 获取Servlet上下文地址
  - `ServletContext  getServletContext()`         获取Servlet上下文地址

#### 共享数据  
  - `void setAttribute(String name,Object obj)`   存储数据
  - `Object geteAttribute(String name)`           通过键从域中获取数据
  - `void removeAttribute(String name)`           通过键从域中删除数据

## Response

### Response继承体系结构
`ServletResponse`接口
`HttpServletResponse`接口继承自`ServletResponse`接口;在`ServletResponse`的基础上新增一些额外的方法

### HttpServletResponse方法

#### 设置响应行
  - `void setStatus(int sc)`                设置状态码
  - `void setStatus(int sc,String message)` 设置状态码及状态码描述

#### 设置响应头
  - `void setHeader(String name,String value)`  设置响应头中的某个参数数据信息

#### 设置响应体
  1. 获取输出流
    - 字符输出流 `PrintWriter getWriter()`
    - 字节输出流 `ServletOutputStream getOutputStream()`
  2. 使用输出流，将数据输出给客户端
    - `输出流对象.write("登录失败");`

#### 设置响应内容类型及编码格式
  - `void setContentType("text/html;charset=utf-8");` 

#### 重定向
  - `response.sendRedirect("重定向的资源路径")`


## 数据共享
Request：
  - 域对象 有作用范围的对象，可以在范围内共享数据
  - 请求域 可以在一次请求的范围内，共享数据
### 方法：
`void setAttribute(String name,Object obj)` 存储数据
`Object geteAttribute(String name)`         通过键从域中获取数据
`void removeAttribute(String name)`         通过键从域中删除数据

## 请求转发与请求重定向

### 转发
  - 在服务端内部的资源跳转方式,转发之后 当前操作的后续代码会执行
  - 实现方式: `request.getRequestDispatcher("要访问的资源地址").forward(request,response)`
    1. `request.getRequestDispatcher("要访问的资源地址")`  获取请求转发对象
    2. `请求转发对象.forward(requst,response)`             执行资源跳转
### 重定向
  - 客户端或服务端都可实现,重定向之后 当前操作的后续代码会执行
  - 实现方式
    - 方式1
      //设置状态码
      `response.setStatus(302);`
      //设置响应头
      `response.setHeader("location","重定向的资源路径")`
    - 方式2
      `response.sendRedirect("重定向的资源路径")`
### 共同点:
  - 转发和重定向都是实现页面跳转
### 区别:
  - 转发时浏览器中的url地址不会发生改变
  - 重定向时浏览器中的url地址会发生改变
  - 转发时浏览器只请求一次服务器
  - 重定向时浏览器请求两次服务器
  - 转发能使用request带数据到跳转的页面
  - 重定向能使用ServletContext带数据到跳转的页面


