---
title: ServletContext详解
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

# ServletContext

## 概述
  - ServletContext代表整个web应用，可以和程序的容器(服务器,例:tomcat)进行通信
  - 整个web应用只有一个ServletContext对象

## 获取
  - Request获取
    - request.getServletContext() 
  - HttpServlet获取
    - this.getServletContext()

## 功能

### 作为全局域来使用(共享数据)
这个域太大,**<font color='red'>慎用</font>**
  - `void setAttribute(String name,Object value)` 设置(修改)数据
  - `Object getAttribute(String name)`  获取数据
  - `void removeAttribute(String name)` 删除数据

### 获取MIME类型
MIME类型：在互联网通信过程中传输的一种文件数据类型
  例: png,jpg 的MIME类型 都是 image/jpeg
  - `String getMimeType(String file)` 获取MIME类型

### 获取文件的真实路径
  - `String getRealPath(String path)` 获取文件编译后的绝对路径

