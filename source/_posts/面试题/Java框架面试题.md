---
title: Java框架面试题
tags:
  - Java面试题
categories:
  面试题
---

## SpringMVC和SpringBoot的区别
SpringBoot包含了Spring和SpringMVC;是它们两个的升级版

## SpringBoot的核心注解
@EnableAutoConfiguration
开启自动化配置
@SpringBootConfiguration
用来声明当前类是SpringBoot应用的配置类，项目中只能有一个
@SpringBootApplication
相当于SpringBoot中的@EnableAutoConfiguration和Spring中的@ComponentScan