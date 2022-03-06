---
title: BeanUtils包
date: 2022-01-03 19:49:57
tags:
  - Java jar包
categories:
  Java
---

# BeanUtils

## 概述
  - apache commons子项目中的软件包
  - 主要目的是利用反射机制对 JavaBean 的属性进行处理

## 作用
  - 简化数据的封装

## 要求
  - 类必须是被public修饰
  - 必须提供无参构造
  - 成员变量必须使用private修饰
  - 提供set和get方法

## 使用
  - `void populate(Object obj,Map map)`	map集合中的内容封装到obj对象中


