---
title: Java修饰符
tags:
  - Java SE基础语法
categories: Java
---

## 概述:
> 是一个状态修饰符。静态的意思。被此修饰符修饰的成员为静态成员。

## 静态:
  1）静态成员不属于对象，会随着类的加载而加载，随着类的销毁而销毁。存在方法区中。
  2）静态成员可以通过类名直接访问
  3）静态成员只能访问静态成员。
  例:
  ``` Java
  public class Demo1 {
    //  静态成员变量
    static String name;

    //  静态成员方法
    public static void test() {    }
  }
  ```

## 代码块：
静态代码块(只会执行一次，在类加载时执行)
构造代码块（每次创建对象都会执行，在构造方法前执行）
局部代码块（主要用于限制变量的作用域，了解）
例:
``` Java
public class Demo1 {
  //  静态代码块
  static { }

  //  构造代码块
  {  }

  public void test2() {
    //局部代码块
    { }
  }
}
```