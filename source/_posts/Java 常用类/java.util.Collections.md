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

## 集合的排序

1. 自然排序
  - 要排序的类必需实现一个接口：`Comparable`，重写该接口中的一个方法`compareTo`方法，在方法中给出排序的依据。
    - 升序:`return 类的属性 - compareTo方法的参数的属性`
    - 降序:`return compareTo方法的参数的属性 - 类的属性`
  - 案例
  ``` Java
  public class Student implements Comparable<Student>{
    private int id;
    private int score;

    //  重写此方法，在此方法中给出排序的依据
    //  排序依据: 先按照成绩升序排序，，如果成绩相同，那么再按照编号降序排序
    @Override
    public int compareTo(Student o) {
      int a = this.score - o.score;
      //如果成绩相同
      if(a == 0){
          a = o.id - this.id;
      }
      return a;
    }
  }

  import java.util.ArrayList;
  import java.util.Collections;
    /*
      自然排序
    */
  public class Run {
    public static void main(String[] args) {
      ArrayList<Student> list = new ArrayList<>();
      list.add(new Student(1,"a张三",99));
      list.add(new Student(2,"c李四",88));
      list.add(new Student(3,"d王五",91));
      list.add(new Student(4,"d呵呵",91));
      list.add(new Student(5,"b赵六",93));
      //  Student实现了`Comparable`并重写`compareTo`方法才可以排序
      Collections.sort(list);
      System.out.println(list)
    }
  }
  ```
2. 比较器排序
  - 在sort方法的第二个参数中给一个`Comparator`接口的实现类对象并让该对象重写`compare()`，该接口是一个比较器接口。（建议使用匿名内部类）
    - 升序:`return 参数1 - 参数2`
    - 降序:`return 参数2 - 参数1`
  - 案例
    ``` Java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Comparator;

    /*
      比较器排序
    */
    public class Run {
      public static void main(String[] args) {
        ArrayList<Student> list = new ArrayList<>();

        list.add(new Student(1,"a张三",99));
        list.add(new Student(2,"c李四",88));
        list.add(new Student(3,"d王五",91));
        list.add(new Student(4,"d呵呵",91));
        list.add(new Student(5,"b赵六",93));
        //  这里使用匿名内部类的方式来实现
        Collections.sort(list, new Comparator<Student>() {
            //给出排序依据即可
            //  升序:参数1 - 参数2
            @Override
            public int compare(Student o1, Student o2) {
                return o1.getScore() - o2.getScore();
            }
        });
        System.out.println(list);
      }
    }
    ```
3. 两种排序方式的比较：
  - 自然排序：
    - 类去实现Comparable接口，重写compareTo方法，在方法中给出排序依据，对类进行排序。
  - 比较器排序：
    - 在sort方法中使用匿名内部类去使用，实现Comparator接口，重写compare方法，在方法中给出排序依据。对集合这个对象进行排序。
  - 比较器排序更加灵活一些。原因：有些类不方便修改时;比如：Integer