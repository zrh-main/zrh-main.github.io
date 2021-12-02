---
title: Arrays
tags:
  - Java重点类
categories:
  Java
---

## 概述:
> 是一个操作数组的工具类。里面成员都是静态的。 位于java.util包

## 常用方法:
> sort()  排序------对原数组进行操作
> toString()  可以把数组中间元素转换为字符串
## 案例
``` Java
public static void main(String[] args) {
    int [] arr = {12,45,12,4,5,48,31,98,1,1,6984,3,1854,1,31,84,631,5,11};
//      排序
    Arrays.sort(arr);
//      转为字符串可输出形式
    System.out.println(Arrays.toString(arr));
}
```