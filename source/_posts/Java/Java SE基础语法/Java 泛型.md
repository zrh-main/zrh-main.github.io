---
title: 泛型
tags:
  - Java SE基础语法
categories: Java
---

## 概述:
  + Java 泛型是 JDK 5 中引入的一个新特性,
  + **<font color='red'>泛型的本质是参数化类型，就是所操作的数据类型被指定为一个参数</font>**
  + 在不创建新的类型的情况下,通过泛型指定的不同类型来控制形参具体限制的类型
  + 操作的数据类型被指定为一个参数,这种参数类型可以用在类,接口,方法中,分别被称为泛型类,泛型接口,泛型方法
  + 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

## java 中泛型标记符：
  + E - Element (在集合中使用，因为集合中存放的是元素)
  + T - Type（Java 类）
  + K - Key（键）
  + V - Value（值）
  + N - Number（数值类型）
  + ？ - 表示不确定的 java 类型

## 定义泛型方法的规则：
  + 所有泛型方法声明都有一个类型参数声明部分（由尖括号分隔），该类型参数声明部分在方法返回类型之前（在下面例子中的 <E\>）。
  + 每一个类型参数声明部分包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。
  + 类型参数能被用来声明返回值类型，并且能作为泛型方法得到的实际参数类型的占位符。
  + 泛型方法体的声明和其他方法一样。注意类型参数只能代表引用型类型，不能是原始类型（像 int、double、char 等）。

## 需求：
写一个排序方法，能够对整型数组、字符串数组甚至其他任何类型的数组进行排序，该如何实现？
  + 答案是可以使用 Java 泛型。
  + 使用 Java 泛型的概念，可以写一个泛型方法来对一个对象数组排序。然后，调用该泛型方法来对整型数组、浮点数数组、字符串数组等进行排序。

  ``` Java
  import java.util.Arrays;
  public class ArrayUtil{

      //  泛型方法;该方法在调用时可以接收不同类型的参数。根据传递给泛型方法的参数类型，编译器适当地处理每一个方法调用
      //  排序
      public static <T> void sort(T[] t) {
          try {
              Arrays.sort(t);
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  }

  import java.util.Arrays;
  public class Run {
      public static void main(String[] args) {

          String[] stringArray = {"d", "a", "c", "b", "e"};
          Integer[] intArray = {3, 43, 64, 2, 43, 45, 67, 9};
          Double[] doubleArray = {23.43, 3.4, 24.4, 4.4, 74.3};
          ArrayUtil.sort(stringArray);
          ArrayUtil.sort(intArray);
          ArrayUtil.sort(doubleArray);
          System.out.println(Arrays.toString(stringArray));
          System.out.println(Arrays.toString(intArray));
          System.out.println(Arrays.toString(doubleArray));
      }
  }
  ```

## 泛型的非限定通配符
``` Java
import java.util.ArrayList;
public class Run {
    public static void main(String[] args) {

      //集合，里面存储字符串
      ArrayList<String> list1 = new ArrayList<>();
      //集合，里面存储整数
      ArrayList<Integer> list2 = new ArrayList<>();
      list1.add("asbgrgh");
      list2.add(15);
      fun(list1);
      fun(list2);
    }

    //  ?是通配符,泛指所有类型的数据
    public static void fun(ArrayList<?> list) {
      System.out.println(list);
    }
}
```

## 泛型的限定通配符
  - <font color='red'>List<? extends T>可以接受任何继承自T的类型的List</font>
  - <font color='red'>List<? super T>可以接受任何T的父类构成的List</font>

  ``` Java
  import java.util.ArrayList;
  public class Run {
      public static void main(String[] args) {

          // String ->  Object               
          //      String 是 Object的子类
          // Integet ->  Number  -> Object    
          //      Integer 是 Number的子类, Number 是 Object 的子类

          //  String  类型集合
          ArrayList<String> stringArrayList = new ArrayList<>();
          //  Integer 类型集合
          ArrayList<Integer> integerArrayList = new ArrayList<>();
            //  Object  类型集合
          ArrayList<Object> objectArrayList = new ArrayList<>();
            //  Number  类型集合
          ArrayList<Number> numberArrayList = new ArrayList<>();

          //  fun1(stringArrayList);
          //  fun1(objectArrayList);
          //  fun1(integerArrayList);
          //  fun1(numberArrayList);
          //  fun2(stringArrayList);
          //  fun2(objectArrayList);
          //  fun2(integerArrayList);
          //  fun2(numberArrayList);
      }
      //  上限    只支持Number及Number的子类
      public static void fun1(ArrayList<? extends Number> list){
          System.out.println("123");
      }
      //  下限    只支持Number及Number的父类,包括Object
      public static void fun2(ArrayList<? super Number> list){
          System.out.println("123");
      }
  }
  ```