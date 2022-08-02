---
title: idea配置
tags:
  - idea
categories: 开发工具
date: 2021-11-15 21:32:03
---

## 给所有项目配置 maven 环境

修改 IDEA 配置的默认的 maven 环境；
File - - - 》 Other Settings - - - 》Settings for New Projects - - - 》Build, Execution, Deployment - - - 》Build Tools - - - 》Maven
![](idea_1.png)
![](idea_2.png)

## 启动配置的命令行太长
Error running 'ApiGatewayZuulApplication': Command line is too long. Shorten command line for ApiGatewayZuulApplication or also for Spring Boot default configuration
### 解决方案
修改项目下 .idea\workspace.xml，找到标签 <component name="PropertiesComponent"> ， 在标签里加一行  <property name="dynamic.classpath" value="true" />