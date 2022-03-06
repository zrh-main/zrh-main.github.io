---
title: 断言
tags:
  - Java SE基础语法
categories: Java
---

## 断言的概念
断言实际上是一种测试机制，它可以规定某个参数或者属性必须要满足某个条件，否则会抛出一个异常，并且程序会中止

## 概述
 在Java中，`assert`关键字，表示断言
 在Java中，assert关键字是从`JAVA SE 1.4` 引入的，为了避免和老版本的Java代码中使用了assert关键字导致错误，`Java在执行的时候默认是不启动断言检查的`（这个时候，所有的断言语句都 将忽略！），如果要开启断言检查，则需要用开关-enableassertions或-ea来开启
　　
## 特点
断言只用于开发测试阶段确定程序的内部错误
断言默认是禁用的，需要手动开启。禁用断言的情况下，类加载器会跳过断言代码
断言检测失败的时候，会抛出AssertionError异常，程序中止
断言可以局部开启的，如：父类禁止断言，而子类开启断言，所以一般说“断言不具有继承性”。
断言只适用复杂的调式过程。
**`断言一般用于程序执行结构的判断，不能让断言处理业务流程`**

## 使用

断言是通过关键字 assert实现的，这个关键字有两种形式
assert 条件 和 assert 条件:表达式
这两种形式都会对条件进行检测，如果结果为false，则抛出一个`AssertionError`异常。
在assert 条件:表达式 这种形式中，表达式会被传入AssertionError的构造器，并将表达式转换成一个消息字符串。
如果条件的检测结果为true，则程序正常运行
**AssertionError是继承自Error，而不是Exception，所以catch用Exception是不能捕捉AssertionError信息的**
## 案例

``` Java
//  判断断言是否开启
public static void main(String args[]) {
    boolean isOpen = false;
    // 如果开启了断言，会将isOpen的值改为true
    assert isOpen = true;
    // 打印是否开启了断言，如果为false，则没有启用断言
    System.out.println(isOpen);
}

//  断言的使用方法一
public static void useAssertExt1() {
    boolean isOk = 4 > 2;
    assert isOk;
    System.out.println("程序正常");
    boolean isOk1 = 1 > 2;
    assert isOk1;
    System.out.println("程序正常");
}
//  执行的结果:
//    1打印程序正常
//    2抛出AssertionError
 
```