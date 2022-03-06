---
title: BigDecimal
date: 2022-2-9 16:56:43
tags:
  - Java 常用类
categories: Java 博客
---

## 简介
用来对超过16位有效位的数进行 **`精确的运算`**
包:`java.math.BigDecimal`
float和double只能用来做科学计算或者是工程计算;在商业计算中要用`BigDecimal`
BigDecimal所创建的是对象，进行数学运算，必须调用其相对应的方法。方法中的参数也必须是BigDecimal的对象

## 构造方法(构造器) 
`BigDecimal(int)`       创建一个具有参数所指定整数值的对象 
`BigDecimal(double)`    创建一个具有参数所指定双精度值的对象(`不推荐使用`)
`BigDecimal(long)`      创建一个具有参数所指定长整数值的对象 
`BigDecimal(String)`    创建一个具有参数所指定以字符串表示的数值的对象(`推荐使用`)

## 方法 
`BigDecimal add(BigDecimal)`         BigDecimal对象中的值相加，然后返回这个对象 
`BigDecimal subtract(BigDecimal)`    BigDecimal对象中的值相减，然后返回这个对象 
`BigDecimal multiply(BigDecimal)`    BigDecimal对象中的值相乘，然后返回这个对象 
`BigDecimal divide(BigDecimal)`      BigDecimal对象中的值相除，然后返回这个对象 
toString()              将BigDecimal对象的数值转换成字符串 
doubleValue()           将BigDecimal对象中的值以双精度数返回 
floatValue()            将BigDecimal对象中的值以单精度数返回 
longValue()             将BigDecimal对象中的值以长整数返回 
intValue()              将BigDecimal对象中的值以整数返回

## BigDecimal(double)不推荐使用的原因

1. 参数类型为double的构造方法的结果有一定的不可预知性;`new BigDecimal(0.1)`所创建的BigDecimal实际上等于0.1000000000000000055511151231257827021181583404541015625;因为0.1无法准确地表示为 double（或者说对于该情况，不能表示为任何有限长度的二进制小数）。这样，传入到构造方法的值不会正好等于 0.1（虽然表面上等于该值）
2. String 构造方法是完全可预知的：写入 newBigDecimal("0.1") 将创建一个 BigDecimal，它正好等于预期的 0.1。因此，比较而言，通常建议优先使用String构造方法
当double必须用作BigDecimal的源时，请使用Double.toString(double)转成String，然后使用String构造方法，或使用BigDecimal的静态方法valueOf

## BigDecimal做等值比较
 使用compareTo 方法