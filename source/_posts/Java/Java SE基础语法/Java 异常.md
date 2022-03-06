---
title: 异常
date: 2021-12-20 16:56:43
tags:
  - Java SE基础语法
categories: Java
---

# 异常

## 概述
- **异常** ：指的是程序在执行过程中，出现的非正常的情况，最终会导致JVM的非正常停止。
- 在Java等面向对象的编程语言中，异常本身是一个类，产生异常就是创建异常对象并抛出了一个异常对象。
- Java处理异常的方式是中断处理。


## 异常体系
异常的根类是`java.lang.Throwable`
子类：
- `java.lang.Error`错误：无法通过代码处理的错误，只能事先避免
- `java.lang.Exception`异常：异常产生后程序员可以通过代码的方式纠正，使程序继续运行


## **Throwable中的常用方法：**

* `void printStackTrace()`:打印异常的详细信息。
    包含了异常的类型,异常的原因,还包括异常出现的位置,在开发和调试阶段,都得使用printStackTrace
* `String getMessage()`:获取发生异常的原因。
    提示给用户的时候,就提示错误原因
* `String toString()`:获取异常的类型和异常描述信息(不用)。

## 异常分类
* **编译时期异常**:checked异常。在编译时期,就会检查,如果没有处理异常,则编译失败。(如日期格式化异常)
* **运行时期异常**:runtime异常。在运行时期,检查异常.在编译时期,运行异常不会编译器检测(不报错)。(如数学异常)

## 异常的处理

Java异常处理的五个关键字：`try、catch、finally、throw、throws`
- 抛异常
  - throw：
    抛出一个异常。
    格式： throw new 异常对象;
  - throws 把异常抛出给调用者去处理；如果到主方法还没有处理，那么就交给jvm去处理

- 捕获异常
使用try...catch..finally格式来捕获异常
格式：
``` Java
try{
  //有可能会出现异常的代码
}catch(){
  //出现异常后执行的操作，出现异常的补救措施
  //catch块可以有多个
}finally{
//一定会执行的代码，一般用于资源释放
}
```
throw和throws的区别（面试题）？
throw：
手动抛出一个异常，一定会抛出。
只能抛一个异常。
在方法体中使用。
throws：
声明一个异常，可能会抛出。
可以抛多个异常。
在方法的声明后使用。


## 自定义异常类
当java给我们提供的异常满足不了我们的需求时，我们可以自定义异常类。
- 编译期异常：
  - 自定义一个类，继承Exception类即可
  - ``` Java
      public class MyException extends Exception{
        public MyException(){}
        public MyException(String name){
            super(name);
        }
      }
    ```
- 运行期异常：
  - 自定义一个类，继承RunTimeException类即可
  - ``` Java
      public class MyException extends RunTimeException{
        public MyException(){}
        public MyException(String name){
            super(name);
        }
      }
    ```

ArrayIndexOutOfBoundsException（数组下标越界异常）
用非法索引访问数组时抛出的异常。如果索引为负或大于等于数组大小，则该索引为非法索引

NullPointerException(空指针异常)
当应用程序试图在需要对象的地方使用 null 时，抛出该异常

ClassCastException(类型转换异常)
试图将对象强制转换为不是实例的子类时，抛出该异常。