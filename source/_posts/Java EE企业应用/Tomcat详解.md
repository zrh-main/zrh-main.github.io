---
title: Java Servlet使用
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

## 安装
  1. 下载：http://tomcat.apache.org/
  2. 安装：解压压缩包即可 * 注意：安装目录中不要有中文和空格
  3. 卸载：删除目录即可

## 目录结构
  - bin			可执行文件
  - config		配置
    - server.xml	tomcat环境配置
  - lib			依赖jar包
  - logs			日志
  - webapps		项目部署目录
  - work			运行时资源

## 服务的启动与关闭
  - 启动服务`bin/startup.bat`	
  - 关闭服务`bin/shutdown.bat` 

## 部署项目的方式：
  1. 直接将项目放到webapps目录下即可
    - 简化部署：将项目打成一个war包，将war包放在webapps目录下
    - war包会自动解压缩

  2. conf/server.xml文件中配置
    - 在<Host>标签中配置<Context docBase=”D:\hello” path=”/hehe” />
      docBase属性：项目的存放的位置
      path属性：虚拟目录
  3. 在conf/Catalina/localhost 创建任意名称的xml文件
    - 在文件中编写：<Context docBase="D:\hello" />
    - 访问路径：`xml文件的名称`

## idea集成开发环境配置tomcat
  - idea会为web项目创建配置文件
  - tomcat启动时会输出配置文件路径  Using CATALINA_BASE
  - 编译打包后的真正文件位于 配置文件的docBase指向的路径
