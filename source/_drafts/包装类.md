---
title: 包装类
tags:
  - Java重点
categories:
  Java
---

## 包装类

概述:
> &nbsp;&nbsp;Java是面向对象的语言,但基本数据类型不是面向对象的;
> &nbsp;&nbsp;在设计时,Java为基本数据类型设计了对应的类(这些和基本数据类型对应的类称为包装类)
> &nbsp;&nbsp;包装类位于java.lang包

对应关系:
> &nbsp;&nbsp;byte->Byte
> &nbsp;&nbsp;boolean->Boolean
> &nbsp;&nbsp;short->Short
> &nbsp;&nbsp;char->Character
> &nbsp;&nbsp;int->Integer
> &nbsp;&nbsp;long->Long
> &nbsp;&nbsp;float->Float
> &nbsp;&nbsp;double->Double

基本类型和包装类型可以直接相互赋值的原因:   这其实是Java中的一种“语法糖”。
 <font color='red'>__“语法糖”是指计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用__</font>
注意: <font color='red'>__定义成员变量时,建议使用包装类来定义__</font>

包装类的主要用途:
1. 作为和基本数据类型对应的类存在,方便涉及到对象的操作
2. 包含每种基本数据类型的相关属性: 最大值,最小值,及相关的操作方法

装箱拆箱
<font color='red'>&nbsp;&nbsp;注：JDK1.5之后提供自动拆装箱的语法，在进行基本数据类型和对应的包装类转换时,系统将自动进行</font>
装箱：将基本数据类型封装为包装类对象
``` Java
Integer i = 10; //装箱，10整数，i是包装类型Integer i = new Integer(10); //编译后
```

拆箱：将包装类中包装的基本数据类型数据取出
``` Java
Integer i = 10; //自动装箱int j = i; //自动拆箱int j = i.intValue(); //编译后
```


