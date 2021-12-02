---
title: Math
tags:
  - Java重点类
categories:
  Java
---

## 概述:
> 数学类。里面成员都是静态的。  位于java.lang包,不需要导入

## 静态成员变量
> PI：圆周率

## 静态成员方法
  abs()  ：绝对值
  round()  :  四舍五入
  ceil()		向上取整
  floor()	向下取整
  random()  随机数

## 案例

``` Java
public static void main(String[] args) {
    //一个接近圆周率的数字,可以用来进行和圆相关的计算
    System.out.println(Math.PI);

    int num = 5;
    //获取num 的绝对值
    System.out.println(Math.abs(num));

    double num2 = 2.635;
    // ceil 向上取整
    System.out.println(Math.ceil(num2));

    double num3 = 6.15;
    // floor 向下取整
    System.out.println(Math.floor(num3));

    double num4 = 5.24;
    //round 四舍五入
    System.out.println(Math.round(num4));

    //返回0-1(包括0,不包括1)之间double类型的随机数
    System.out.println(Math.random());
    
    //返回1-101之间的随机数
    System.out.println( (int)(Math.random()*100) + 1);
}
```