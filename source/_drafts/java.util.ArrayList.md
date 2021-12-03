---
title: ArrayList
tags:
  - Java常用类
categories:
  Java
---

## ArrayList

概述: 
    ArrayList 类是一个可以动态修改的数组，它没有固定大小的限制，可以添加或删除元素。
    ArrayList 继承了 AbstractList ，并实现了 List 接口
    ArrayList 是一个数组队列，提供了相关的添加、删除、修改、遍历等功能
    <font color='red'>__实现了基于动态数组的数据结构&nbsp;&nbsp;查询操作效率高&nbsp;&nbsp;插入和删除操作效率低__</font>

使用: 
1. 导包:   ```import java.util.ArrayList;``` 
2. 创建:   ```ArrayList<E> objectName =new ArrayList<>();　 // 初始化```
3. 常用方法: ```objectName.add();objectName.remove();objectName.size();objectName.get()```

案例:
``` Java
import java.util.ArrayList;
public class Demo {
    public static void main(String[] args) {
        //创建 String 类型 的集合对象
        ArrayList<String> list = new ArrayList<>();
        // 添加元素
        list.add("杨过");
        //根据索引删除，返回被删除的元素
        list.remove(0);     
        //根据元素删除，返回成功或者失败
        list.remove("杨过"); 
        //获取集合的长度大小
        System.out.println("集合列表的长度为:"list.size())
        //获取集合中索引为0的元素
        System.out.println("集合列表中索引为0的元素内容是:"list.get(0))
    }
}
```
