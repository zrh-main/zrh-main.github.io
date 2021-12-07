---
title: String
tags:
  - Java常用类
categories:
  Java
---

### 概述:
> 是和字符串相关的类。  位于java.lang包 
> 使用final修饰(最终类)

### StringBuilder类与StringBuffer类
字符串拼接的 + 在底层为StringBuilder类的append()
append(String):在原有字符串拼接
StringBuffer类是同步的,因此耗时,所以出现了StringBuilder类

### 构造方法:
* new String() 
  *  初始化一个新创建的 String 对象，使其表示一个空字符序列。
* new String("abc") 
  *  初始化一个新创建的 String 对象，使其表示一个与参数相同的字符序列

### 内存
字符串常量池和运行时常量池逻辑上属于方法区，但是实际存放在堆内存中

### 常见问题(面试题):
* String s = “hello” 和String s = new String(“hello”)区别
  * 内存分配:
      * 栈区
      * 堆区:
        * 常量池
      * 方法区
  * String s = "hello"
    * (常量池不存在hello时)在常量池开辟空间存放hello生成地址,栈区中的s变量指向hello生成的地址
    * (常量池存在hello时)栈区中的s变量指向hello的地址
  * String s = new String("hello")
    * (常量池不存在hello时)在常量池开辟空间存放hello并生成地址,堆区中开辟空间指向hello生成的地址,栈区中的s变量指向堆区中的地址
    * (常量池存在hello时)堆区中开辟空间指向hello生成的地址,栈区中的s变量指向堆区中的地址
* equals和==的区别:
  * 基本数据类型中的==比较的是值是否相同
  * 引用数据类型:
    * ==比较的是内存中的地址值是否相同
    * equals默认继承object的情况下比较的还是内存中的地址值,String 这里的equals比较的是字符串的内容是否相同
  * String s = “hello”,s += “world”，变量s的值变了吗？
    * s的值变为helloworld;但是内存中的常量池中还是存在hello;只是栈区中的s变量指向改为了helloworld

### 常用方法

#### 案例如下
``` Java
public static void main(String[] args) {

  //  equals  比较两个字符串的内容是否相同
  // 这里的str1直接指向常量池中"123"的地址值
  String str1 = "123";
  // 这里的str2指向堆中new String("123")的地址值
  String str2 = new String("123");
  // 这里是栈区中str1变量所指向的地址,和堆中new String("123")的地址进行比较,所以不相同
  if (str1 != str2) {
      System.out.println("==比较堆中的地址值不相同");
  }
  if (str1.equals(str2)) {
      System.out.println("equals比较内容相同");
  }

  //  length    获取字符串的长度
  String str3 = "123456";
  //获取字符串str3的长度
  System.out.println(str3.length());

  //  charAt   获取指定索引位置的字符
  String str4 = "123456";
  System.out.println(str4.charAt(0));

  //  indexOf  获取指定字符或字符串在当前字符串中首次出现的位置,没找到返回-1
  String str5 = "123456789";
  System.out.println(str5.indexOf(3));

  //  split    将字符串以某个字符拆分成字符串数组,字符串拆分
  String str6 = "2021-12-2";
  //接收的参数为正则,需注意特殊符号,以特殊符号拆分可以添加在[ ]之内
  System.out.println(str6.split("[-]"));

  //  substring    截取字符串,并返回新的字符串
  String str7 = "1十分1吖1放啊东方1阿发吖131516发sasafafasdfas";
  //截取索引从0-5之间(包括0,不包括5)的字符,
  System.out.println(str7.substring(5));
  //截取索引从2-12之间(包括2,不包括12)的字符
  System.out.println(str7.substring(2, 12));

  //  concat 将指定字符串拼接到此字符串的末尾
  //  参数长度为0时,返回此字符串对象;否则返回新的String对象
  String str8 = "123"
  System.out.println(str8.concat("abc"));

}
```