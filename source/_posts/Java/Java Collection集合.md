---
title: Collection集合
date: 2021-12-09 10:58:39
tags:
  - Java重点
categories:
  Java
---

## Collection集合

### 概述:
- 集合是java中提供的一种容器，可以用来存储数据
- 包: `java.util.Collection`
- 类型: 接口

### 集合和数组的区别(面试题)？
- 数组的长度是固定的，集合的长度是可变的
- 数组中只能存储同一种数据类型，集合中可以存储不同的数据类型
- 数组中可以存储基本数据类型也可以存储引用数据类型;集合中只能存储引用数据类型（集合存储基本数据类型其实存储的是对应的包装类）

### 子接口
- `java.util.List`和`java.util.Set`;
- `List`的特点是有序、可重复
- `Set`的特点是无序，不可重复。

### 子接口实现类
- `List`接口的常用实现类有`java.util.ArrayList`和`java.util.LinkedList`
- `Set`接口的常用实现类有`java.util.HashSet`,`java.util.TreeSet`,`java.util.LinkedHashSet`

### Collection 常用方法
Collection是父接口，因此在Collection中定义了(List和Set)通用的一些方法，这些方法可用于操作所有的单列集合
* `boolean add(E e)`：      添加元素
* `void clear()` :          清空元素
* `boolean remove(E e)`:    删除元素
* `boolean contains(E e)`:  判断集合中是否包含该元素
* `boolean isEmpty()`:      判断集合是否为空
* `int size()`:             返回集合中元素的个数
* `Object[] toArray()`:     集合转为数组
* `Iterator<E> iterator()`: 创建迭代器对象

### Collection 常用方法案例
``` Java
public class Student {
    private String name;
    private Integer age;
    private Double score;

    public Student() {}
    public Student(String name, Integer age, Double score) {
        this.name = name;
        this.age = age;
        this.score = score;
    }
    @Override
    public String toString() {
        return "Student{"+"name='" + name + '\'' +", age=" + age +", score=" + score +'}';
    }
}


import java.util.ArrayList;
import java.util.Collection;
public class CollectionDemo {
    public static void main(String[] args) {

        //创建一个集合
        Collection coll = new ArrayList();
        Student student = new Student("张三", 19, 85.6);

        //  add  添加
        coll.add(student);
        coll.add(18);
        coll.add('@');
        coll.add(true);
        coll.add(185.45);
        System.out.println(coll);

        //  remove  删除
        //  coll.remove(student);
        System.out.println(coll);

        //  contains    判断集合中是否存在该元素
        //  System.out.println(coll.contains(student));

        //  clear   清空
        coll.clear();
        //  System.out.println(coll);

        //  isEmpty  判断集合是否为空
        System.out.println(coll.isEmpty());
        
        //  toArray 集合转数组
        Object[] objects = coll.toArray();

        //创建迭代器对象
        Iterator iterator = coll.iterator();
    }
}

```

## 迭代器

### 概述:
- 主要用于迭代访问（即遍历）`Collection`中的元素，因此`Iterator`对象也被称为迭代器
- 包: `java.util.Iterator`
- 类型: 接口

### 创建迭代器
Collection集合中有一个方法`Iterator<E> iterator()`
作用是获取集合对应的迭代器对象，用来遍历集合中的元素
``` Java
//创建迭代器对象
Iterator iterator = 集合对象.iterator();  
```

### 迭代器常用方法
- `boolean hasNext()`:  判断集合的下一个位置是否有元素
- `E next()`:           <font color='red'>**获取当前指向的元素,并指向下一个元素**</font>此方法使用时需注意!!! 			
- `void remove()`:      删除所指向的元素

### 常用方法案例:

``` Java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class CollectionDemo {
  public static void main(String[] args) {
    //创建集合
    ArrayList<String> stringArrayList = new ArrayList<>();
    stringArrayList.add("1");stringArrayList.add("2");

    //创建迭代器对象
    Iterator<String> iterator = stringArrayList.iterator();

    // 使用迭代器遍历集合
    while (iterator.hasNext()) {//判断集合的下一个位置是否有元素
    //获取当前指向的元素,并指向下一位元素;   注意:这里如果没有执行next();将陷入死循环中!!!!!!!!!!!!!!
        String s = iterator.next();
        if (s.equals("1")) {
            iterator.remove();//删除当前指向的元素
        }
    }
  }
}
```

## 增强for
  - 增强for是jdk1.5的新特性。
  - 作用是：可以遍历数组和集合。
  - 内部原理是个Iterator迭代器，所以在遍历的过程中，不能对集合中的元素进行增删操作

### 细节：
  - <font color='red'>用于遍历元素使用，不能使用增强for删除元素</font>

### 格式：
for(元素类型 元素名: 集合名 ){
  System.out.println(元素名)
}

### 案例:

``` Java
import java.util.ArrayList;
import java.util.Iterator;
public class Demo {
    public static void main(String[] args) {
        ArrayList<String> list  = new ArrayList<>();
        list.add("张三");
        list.add("李四");

        //打印集合元素
        for (String s : list) {
            System.out.println(s);
        }

    }
}
```