---
title: 其它
date: 2022-2-9 16:56:43
tags:
  - Java SE基础语法
categories: Java
---

## lambda表达式
### 函数式编程思想
面向对象：根据多个对象中共有的属性和行为抽取出一个类，类去创建对象，对象调用方法。
函数式编程思想：
不去关心是哪个对象去做的，不关心怎么做的，只需要一个输入值，然后得到一个结果。
在java中函数式编程思想的体现就是lambda表达式。
可以把lambda表达式当作匿名内部类的简化版格式。（底层其实有一些区别）
* 当接口作为方法的参数时，我们实际要传入一个接口的实现类的对象，之前我们使用匿名内部类来简化这	个过程，但是匿名内部类的格式过于繁琐，不易阅读，所以，在jdk1.8时提供了lambda的语法在某些情况	下可以替代掉匿名内部类。
2.使用前提
1）接口必须是函数式接口（接口中只能有一个抽象方法）。
2）要具有上下文推断。
### 格式：
`()->{}`
()：接口中抽象方法的参数
->：没有特殊含义，就是一个传递作用,
{}：抽象方法的方法体的重写。
### 省略规则
  1. 小括号中方法的参数类型可以省略。
  2. 当小括号中只有一个参数时，小括号可以省略。
  3. 当方法体中只有一条语句时，return，语句后的分号，{}都可以省略。

## 集合的排序

### 自然排序
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
### 比较器排序
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
### 两种排序方式的比较：
  - 自然排序：
    - 类去实现Comparable接口，重写compareTo方法，在方法中给出排序依据，对类进行排序。
  - 比较器排序：
    - 在sort方法中使用匿名内部类去使用，实现Comparator接口，重写compare方法，在方法中给出排序依据。对集合这个对象进行排序。
  - 比较器排序更加灵活一些。原因：有些类不方便修改时;比如：Integer

## 循环性能分析

多种循环方式，迭代器、for循环、forEach循环、lambda的forEach循环之间的性能问题...
### ArrayList
 当size很大（10万）时候for循环的性能最高，当size不是很大的时候Iterator和forEach效率就高
 如何选择可参考
  - 如果是需要对下标进行处理操作对则选择for循环；
  - 对于ArrayList，从代码简洁讲优先考虑foreach，也不用去考虑下标越界对问题；
  - 对于实效性要求不高的，如I/O操作，可考虑lambda的forEach循环。

### LinkedList
 此时Iterator性能最佳，由于LinkedList的数据结构为链表结构，其中每个节点都记录了前驱和后继结点（头、尾除外）。
 通过下标的方式获得值都会在链中检查一遍，直到得到我们想要的下标节点为止。
 而Iterator和forEach则是始终循环子节点，直到尾节点为止。考虑到代码简洁，建议使用forEach。

### HashMap
Iterator循环始终效率方面要占优，这里简单的提下HashMap的放值和取值原理。

  HashMap是一个散列桶，由数组和链表组成。每次put操作都会得到key的hash值，然后根据得到的hash值选择map数组对应的下标存入bucket（存入键对象和值对象）。而get操作时是根据键对象的hash值找map数组中对应的bucket，然后得到对应的值（当存在hash碰撞时，会以链表的形式加在相同hash值的后面，每次get的时候调用keys.equals（）得到该键与之对应的值）

  在之前的List对比中已经说明forEach循环底层也是由Iterator实现，而lambda的forEach循环是充分利用cup实现多线程的迭代。所以不难得出HashMap遍历Iterator效率更高一点，但是基于代码简洁性考虑也可采用forEach循环方式。

## 路径
绝对路径：
以磁盘为起点的路径。
相对路径：
以项目的根目录为起点的路径。

## 递归
### 概述
  递归是指方法自己调用自己的现象。
### 分类
- 直接递归
方法自己直接调用自己的现象。
- 间接递归
方法通过别的方法调用自己的现象。
### 注意事项（*）
1. 递归要有结束条件，否则栈内存溢出。
2. 递归的次数不能过多，否则栈内存溢出。
3. 构造方法不能递归。
### 递归的过程：
递推阶段
回归阶段
### 递归算法的编写思路
什么情况下可以使用递归思想？
如果一个大的问题可以被分解成若干个小问题，而且若干个小问题的解决思路	差不多，那么可以考虑使用递归思想。	
怎么编写递归？
1. 编写递归结束条件。
2. 递归调用时让参数向结束条件靠拢。
3. 找到本次递归与上一次递归之间的关系，描述这个关系。

### 文件递归案例
#### 递归打印多级目录
打印出abc目录下的所有内容：如果子目录中有内容，也需要打印出来
思路：
1）获取出abc中的所有内容，存储到一个文件数组中
2）遍历数组，判断每一个元素是文件还是目录
文件
直接打印即可。
目录
获取出该目录中的所有内容，存储到一个文件数组中。再次进行2	步骤即可。
``` Java
/*
    递归打印多级目录
 */
public class Test1 {
    public static void main(String[] args) {
        File file = new File("D:\\abc");
        //调用打印目录的方法
        show(file);
    }
    //打印目录的方法
    public static void show(File file){
        //取出该目录下的内容，存储到数组中
        File[] files = file.listFiles();
        //遍历该数组，判断是文件还是目录
        for (File file1 : files) {
            if(file1.isDirectory()){//目录
                System.out.println(file1);    //打印出目录名字
                show(file1);
            }else{  //是文件
                System.out.println(file1);
            }
        }

    }
}
```
#### 文件搜索案例
搜索出abc目录下所有.mp3结尾的文件。打印出来。
思路：
1）获取出abc中的所有内容，存储到一个文件数组中
2）遍历数组，判断每一个元素是文件还是目录
文件
判断文件名是否以.mp3结尾，是直接打印即可。
目录
获取出该目录中的所有内容，存储到一个文件数组中。再次进行2	步骤即可。
``` Java
public class Test2 {
    public static void main(String[] args) {
        File file = new File("D:\\abc");
        sou(file);
    }

    //搜索功能：搜索以.mp3结尾的文件
    public static void sou(File file) {


        File[] files = file.listFiles();
        for (File file1 : files) {
            if(file1.isDirectory()){
                sou(file1);
            }else{//文件
                if(file1.getName().endsWith(".txt")){
                    System.out.println(file1);
                }
            }
        }
    }
}
```
#### 文件过滤器优化
FileFilter：
文件过滤器的一个接口，该接口中有一个方法，可以过滤掉不需要的文件。
要求：返回值：true，表示保留
false：表示过滤。
``` Java
public class Test3 {
    public static void main(String[] args) {
        File file = new File("D:\\abc");
        sou(file);
    }

    //搜索功能：搜索以.mp3结尾的文件
    public static void sou(File file) {
        File[] files = file.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                //保留目录和以.mp3结尾的文件
                return pathname.isDirectory() || pathname.getName().endsWith(".mp3");
            }
        });
        for (File file1 : files) {
            if(file1.isDirectory()){    //如果是目录
                sou(file1);
            }else{
                //如果是文件
                System.out.println(file1);
            }

        }


    }
}

```
#### 斐波那契数列
``` Java
/*
    斐波那契数列：
        1    1    2     3      5      8       13       21       34...
        1    2    3     4      5      6        7        8       9
            除了前两个数字之外，其它的数字都是由上一个数字和上上一个数字的和组成的。

        1.编写递归结束条件。
        2.递归调用时让参数向结束条件靠拢。
        3.找到本次递归与上一次递归之间的关系，描述这个关系。
 */
public class Test5 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个编号：");
        int n = sc.nextInt();
        System.out.println("结果为：" + fei(n));
    }
    //求出编号对应的费事数列
    public static int fei(int n){
        //结束条件
        if(n == 1 || n == 2){
            return 1;
        }
        return fei(n - 1) + fei(n - 2);
    }
}

```



