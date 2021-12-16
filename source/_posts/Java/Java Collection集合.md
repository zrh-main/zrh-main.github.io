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

## 子接口详解

### List接口

  - 位于`java.util.List`包
  - 特点:有序，有索引，可重复
  - 子类
    - `java.util.ArrayList`
      - 结构:数组结构
      - 特点:元素增删慢，查询快
    - `java.util.LinkedList`
      - 结构:链表结构
      - 特点:元素查询慢,增删快
    - `Vector`

### Set接口

  - 位于`java.util.Set`包
  - 特点:没有索引，不可重复
  - 子类
    - `java.util.HashSet`
      - 结构:哈希表
      - 特点:元素无序，不可重复
    - `java.util.LinkedHashSet`    
      - 结构:哈希表+链表
      - 特点:元素有序，不可重复
    - `java.util.treeSet`
      - 结构:红黑树
      - 特点:查询速度非常快

### 哈希表

  - 在jdk8之前，底层是数组+链表。
  - 在jdk8之后，底层是数组+链表/红黑树（当同一个哈希值对应的元素多于7个，会自动的把链表变成红黑树）

#### 哈希冲突
  - 两个没有任何联系的字符串的哈希值竟然一样，这个现象叫哈希冲突。

#### set集合的去重原理：
  1. 先使用哈希值进行对比，如果没有一样的，直接存储  hasCode()
  2. 如果有哈希值一样的，那么会用equals方法来比较内容，如果内容不同，存		储，如果内容相同，说明存在，不存储
 ![](哈希表.png)

#### 注意：
  - 用set集合来存储元素，如果存储自定义的数据类型，那么类中必须重写equals和hasCode方法，来保证set集合去重。

### 可变参数：
  - 在jdk1.5后，有可变参数的新特性。

#### 格式：
修饰符 返回值类型 方法名(参数类型... 形参名){  }

#### 注意：
  1. 可变参数的本质是一个数组。
  2. 如果一个方法有多个参数，但只能有一个可变参数，可变参数必须是最后一个参数。
  3. 如果一个方法的参数中有可变参数，那么可以不传递可变参数。

#### 案例
``` Java
/*
  可变参数
  修饰符 返回值类型 方法名 (int... a){}
  当我们方法传递参数的时候，我们不知道传递几个，可使用可变参数
 */
public class Test1 {
    public static void main(String[] args) {
        getSum(3,5,11,8);
    }

    //求和
    public static void getSum(int... arr){
        System.out.println(arr.length);
        System.out.println(arr[0]);
        System.out.println(arr[1]);
        System.out.println(arr[2]);
        System.out.println(arr[3]);
}
```

