---
title: 基于xml配置
tags:
  - Java 框架
categories:
  - Java
---

## 概述
xml 配置和注解配置 实现的功能都一样 
都是要降低程序间的耦合。只是配置的形式不一样 
两种配置方式都需要掌握

## spring 的xml配置

## xml版本号及编码格式
**必须**
``` xml
<?xml version="1.0" encoding="UTF-8"?>
```

## beans spring配置根元素标签
**必须**,配置约束信息
``` xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```
## context:component-scan 包扫描
作用:
  spring容器扫描该配置下的类,添加到spring容器中;多个bean时使用此标签
**beans标签的子标签**
属性
  base-package spring容器扫描的包路径;spring容器会将该路径下的bean添加到容器中

使用:
多个路径时,使用 `,`分隔
``` xml
  <context:component-scan base-package="cn.dy"></context:component-scan>
```

## bean  spring配置bean对象标签
bean 标签：**beans标签的子标签**
作用： 
用于配置bean对象，并且存入 ioc 容器之中;
**默认情况下它调用的是类中的无参构造函数。如果没有无参构造函数则不能创建成功** 
属性： 
id：给对象在容器中提供一个唯一标识。用于获取对象。 
class：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。 
scope：指定对象的作用范围。 
* singleton :默认值，单例的. 
* prototype :多例的. 
* request :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中. 
* session :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中. 
* global session 
WEB 项目中,应用在 Portlet 环境.如果没有 Portlet 环境那么 
globalSession: 相当于 session. 
init-method：指定类中的初始化方法名称。 
destroy-method：指定类中销毁方法名称。 

##  property
标签位置：bean标签的内部
属性：
name：用于指定给构造方法中指定名称的参数赋值。
value：用于提供基本类型和String类型的数据
ref：用于指定其他bean的数据类型。指的是spring的IOC核心容器中出现过		的bean对象。
优势：
创建对象时没有明确限制，可以直接使用默认构造方法。
弊端：
如果某个成员必须有值，获取对象时有可能set方法没有执行。

使用
``` xml
 <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
```
##  constructor-arg
使用
``` xml
<constructor-arg name="ds" ref="dataSource"></constructor-arg>
```





