---
title: Objects
tags:
  - Java常用类
categories:
  - Java
---

# Object
1. 概述:
  - 在**JDK7**添加了一个Objects工具类
  - 它提供了一些方法来操作对象

2. 常用方法:
  - boolean equals()
    Object的equals()容易抛出空指针异常，而Objects类中的equals方法就优化了这个问题
    ``` Java
    //Objects的equals();实际还是调用了Object的equals();只是加了一些非空判断...
    public static boolean equals(Object a, Object b) {  
      return (a == b) || (a != null && a.equals(b));  
    }
    ```