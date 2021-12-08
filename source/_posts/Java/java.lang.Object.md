---
title: Object
tags:
  - Java常用类
categories:
  - Java
---

# Object

## 概述:
  - `java.lang.Object`类是Java语言中的根类，即所有类的父类
  - 没有使用extends 继承父类的类都默认继承Object
  - 所有的类都可以使用Object类中的方法

## 常用方法:
  - String toString()
      返回该对象的字符串表示;其实该字符串内容就是 `对象的类型+@+内存地址值`
      <font color='red'>Java中直接打印对象,实际调用了对象的toString()</font>
  - boolean equals()
      比较两个对象的地址值是否相同;
      这里推荐使用Objects的equals(); Object的equals()容易抛出空指针异常，而Objects类中的equals方法优化了这个问题

## 常用方法的使用
  - toString()
    由于toString()返回的是内存地址，而在开发中，需要按照对象的属性得到相应的字符串表现形式，因此可以重写它
    在IntelliJ IDEA中，可以点击`Code`菜单中的`Generate...`，也可以使用快捷键`alt+insert`，点击`toString()`选项;选择需要包含的成员变量并确定进行自动代码生成
  - equals()
    如果希望进行对象的内容比较，即所有或指定的部分成员变量相同就判定两个对象相同，则可以覆盖重写equals方法
    在IntelliJ IDEA中，可以使用`Code`菜单中的`Generate…`选项，也可以使用快捷键`alt+insert`，并选择`equals() and hashCode()`进行自动代码生成

## 面试题
  == 和 equals的区别？
  区别:
    ==  
      基本类型:比较内容是否相同
      引用类型:比较地址值是否相同
    equals
      只能比较引用类型;默认继承Object情况下比较地址值是否相同;
      Object中的equals()是用==比较的