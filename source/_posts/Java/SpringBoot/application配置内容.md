---
title: application配置内容
tags:
  - Java 框架
categories:
  - Java
---

## 概述
SpringBoot 默认使用以下 2 种全局的配置文件，其文件名是固定的。
`application.properties`或`application.yml`

## yml介绍
application.yml 是一种使用 YAML 语言编写的文件，它与 application.properties 一样，可以在 Spring Boot 启动时被自动读取，修改 Spring Boot 自动配置的默认值。


## 端口号
默认端口号为8080
server.port=`8848`

## 日志
logging.level.org.springframework=`debug`

## 数据源
spring.datasource.url=`jdbc:mysql://localhost:8080/db1`
spring.datasource.username=`root`
spring.datasource.password=`root`

## mybatis
SpringBoot没有整合mybatis;mybatis提供了SpringBoot的整合方式
mybatis.type-type-aliases-package=`com.dy.entity`


## 注入配置文件内容方式
``` Java
@Value("server.port")
private Integer port
```