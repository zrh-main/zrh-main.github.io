---
title: Map集合
date: 2021-12-17 10:27:40
tags:
  - Java重点
categories:
  Java
---

# Map

## 概述:
- Map集合是java中提供的一种容器，可以用来存储键值对数据
- 包: `java.util.Map`
- 类型: 接口

## 特点
- 元素由键和值构成。左边叫键，右边叫值
- 键无序，不可重复。值可以是任意类型,任意值
- set集合的底层使用的就是map集合的键

## 常用实现类
- HashMap：底层是哈希表，键无序，不可重复。
- LinedHashMap：底层是哈希表+链表，键有序，不可重复。
- TreeMap：底层是红黑树，查询效率高
- Hashtable

## HashMap和Hashtable的区别(面试题)？
- HashMap：1.2版本出现。线程不安全，效率高。允许null作为键和值。
- Hashtable：1.0版本出现。线程安全，效率低。（最早的双列集合）不允许null作为键和值

## 常用方法
- `V put(K key, V value)`
  - 添加元素，如果键存在，那么新值会替换掉旧值。返回旧值。
- `V remove(Object key)`
  - 根据键删除键值对，返回被删除的值。如果键不存在，返回null。
- `V get(Object key)`
  - 根据键获取值，如果键不存在，返回null。
- `boolean containsKey(Object key)`
  - 判断集合中是否包含该键，如果不包含，返回false。
- `Set<K> keySet()`
  - 获取map集合中的所有的键，存储到一个set集合中
- `Set<Map.Entry<K,V>> entrySet()`
  - 获取map集合中所有的键值对对象，存储到一个set集合中键值对对象

## 案例
``` Java
import java.util.HashMap;
import java.util.Map;
public class Run {
    public static void main(String[] args) {

        //  创建(键值对集合)
        Map<String,Integer> stringIntegerMap = new HashMap<>();
        map.put("杨过","小龙女");
        map.put("郭靖","黄蓉");
        map.put("小明","小红");

        //  添加元素
        stringIntegerMap.put("1",22);
        stringIntegerMap.put("2",18);
        stringIntegerMap.put("3",32);
        stringIntegerMap.put("4",42);
        stringIntegerMap.put("5",12);
        stringIntegerMap.put(null,null);

        //  通过键获取值
        Integer integer = stringIntegerMap.get(null);
        System.out.println(integer);

        //  判断集合中是否包含该键
        boolean b = stringIntegerMap.containsKey(null);
        System.out.println(b);

        //  keySet()返回Set集合,其中是所有的key
        Set<String> strings = stringIntegerMap.keySet();
        for (String string : strings) {
            System.out.println(string);
            stringHashMap.get(string);
        }

        //  entrySet()返回键值对集合
        Set<Map.Entry<String, Integer>> set = map.entrySet();
        //  遍历键值对对象集合
        for (Map.Entry<String, Integer> entry : set) {
            //获取键
            String key = entry.getKey();
            //获取值
            String value = entry.getValue();
            System.out.println(key + "-" + value);
        }
    }
}

```