---
title: Collections
date: 2021-12-16 19:17:09
tags:
  - Java常用类
  - Java集合
categories:
  Java
---
# Collections

## 概述
  - 操作单列集合（Collection）的工具类

## Collection和Collections的区别(面试题)？
  - Collection是单列集合的根接口。
  - Collections是工具类。

## 常用方法
  - static <T> boolean `addAll(Collection<T> c, T... elements) `
    - 批量添加元素
  - static void `shuffle(List<?> list)`
    - 打乱集合中的元素顺序。
  - static <T> void `sort(List<T> list)`
    - 给集合中的元素排序。
  - static <T> void `sort(List<T> list，Comparator<? super T>)`
    - 给集合中的元素排序。

## 案例

``` Java
import java.util.ArrayList;
import java.util.Collections;

/*
  addAll(Collection<T> c, T... elements)
    批量添加元素
  shuffle(List<?> list)
    打乱集合中的元素顺序。
 */
public class Test3 {
    public static void main(String[] args) {
        //创建集合
        ArrayList<String> list = new ArrayList<>();

//        list.add("张飞");
//        list.add("王菲");
//        list.add("刘亦菲");
//        list.add("胡一菲");
        Collections.addAll(list,"张飞","王菲","胡一菲","贵妃");
        Collections.shuffle(list);
        System.out.println(list);
    }
}
```