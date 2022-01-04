---
title: 环境
tags:
  - Java SE基础语法
categories: Java
date: 2021-12-02 14:09:58
---

## 虚拟机——JVM
* 概述:
  * JVM（Java Virtual Machine ）: Java虚拟机，简称JVM，是Java程序的运行环境，是Java 最具吸引力的特性之一。 
* 特性：
  * 跨平台: 软件的运行，都必须在操作系统之上，而Java编写的软件可以运行在任何操作系统上，这个特性称为Java**语言的跨平台特性**。该特性是由JVM实现的，我们编写的程序运行在JVM上，而JVM 运行在操作系统上。
 ![](Java虚拟机.png)

## 运行时环境——JRE
* 概述:
  * JRE: Java程序的运行时环境,包含JVM和运行时需要的核心类库

## 开发环境——JDK
* 概述:
  * JDK: Java程序的开发工具包,包含JRE和开发使用的工具
* 安装:
  * Windows10搭建Java环境
    1. 卸载
      打开控制面板
        ->卸载程序
        ->Java(Tm) 	右键卸载
        ->Java		右键卸载
    2. 安装
      安装开发工具,修改安装路径
      出现jre的安装,点击 X 取消安装
    3. 环境变量->系统变量
      JAVA_HOME(没有则新增)		D:\developer\Java\jdk1.8.0_144(jdk目录)
      CLASSPATH(没有则新增)	.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
      Path(没有则新增)		%JAVA_HOME%\bin
    4. 测试环境是否配置成功
        cmd下运行Java -version
        ``` bash
        Java -version
        ```

## 关系图
![](Java环境关系图.png)

<font color='red'>__运行Java程序,只需安装JRE!!!__</font>
<font color='red'>__开发Java程序,必须安装JDK!!!__</font>

