---
title: Date
tags:
  - Java常用类
  - Java时间日期类
categories:
  - Java
---

# Data

## 概述:
位于`java.util`;类 Date 表示特定的瞬间，精确到毫秒; 1秒 = 1000毫秒

## 构造方法
 Date()
  表示当前的时间
 Date(long date)
  表示从标准基准时间（即 1970 年 1 月 1 日 00:00:00 GMT）+ 指定毫秒数(date)的时间

## 常用方法
  long getTime()
    表示时间对象所对应的毫秒值