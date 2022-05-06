---
title: service的支持
tags:
  - MybatisPlus
categories:
  - Java
---

## 概述
MybatisPlus提供了对Service的支持
Service的接口继承Iservice,Service的实体类继承ServiceImpl即可使用MybatisPlus提供的方法

## Iservice<T>
service接口继承Iservice<pojo实体类>
Iservice内部提供了一些单表操作方法

## ServiceImpl<mapper接口.class,pojo实体类>
service实现类继承ServiceImpl<mapper接口,pojo实体类>即可使用