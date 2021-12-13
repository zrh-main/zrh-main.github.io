---
title: Stream
categories: Java
date: 2021-12-10 10:31:16
tags:
  - Java常用类
---
## 流
- 将要处理的元素集合看作一种流，流在管道中传输，并且可以在管道的节点上进行处理，比如筛选，排序，聚合等。
- 元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果。

## 聚合操作
- 聚合操作 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

## 概述
- Stream是Java 8提供的新功能，是对集合（Collection）对象功能的增强，能对集合对象进行各种非常便利、高效的聚合操作- （aggregate operation），或者大批量数据操作 (bulk data operation)。
- Stream流其实是一个集合元素的函数模型，不是集合，也不是数据结构，其本身并不存储任何元素（或其地址值），Stream是一个来自数据源的元素队列。它是有关算法和计算的，它更像一个高级版本的 Iterator。
- 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
- 数据源 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
- 同时它提供串行和并行两种模式进行汇聚操作，并发模式能够充分利用多核处理器的优势，使用 fork/join 并行方式来拆分任务和加速处理过程。
- 包:`java.util.stream`接口
- 版本: `1.8`开始支持
- 同类型接口:`IntStream`,`LongStream`,`DoubleStream`

## Stream的优势
- 与Lambda 表达式结合，也可以提高编程效率、简洁性和程序可读性。
- 通常编写并行代码很难而且容易出错, 但使用 Stream API 无需编写一行多线程的代码，就可以很方便地写出高性能的并发程序。
- 高级版本的 Stream，用户只要给出需要对其包含的元素执行什么操作，比如 “过滤掉长度大于 10 的字符串”、“获取每个字符串的首字母”等，Stream 会隐式地在内部进行遍历，做出相应的数据转换。

### Stream的优势案例
遍历姓“花”的到集合A，再找出名字是3个字的到集合B，最后遍历输出B中的元素。
``` Java
import java.util.ArrayList;
import java.util.List;

public class Demo1 {
    public static void main(String[] args) {
        // 传统的做法：
        List<String> list = new ArrayList<>();
        list.add("花和尚");
        list.add("花豹子");
        list.add("花花公子");
        List<String> listA = new ArrayList<>();
        for (String s : list) {
            if (s.startsWith("花")) {
                listA.add(s);
            }
        }
        List<String> listB = new ArrayList<>();
        for (String s : listA) {
            if (s.length() == 3) {
                listB.add(s);
            }
        }
        for (String s : listB) {
            System.out.println(s);
        }
        // 使用stream的方式:
        List<String> listC = new ArrayList<>();
        list.stream().filter(item -> item.startsWith("花")).filter(item -> item.length() == 3).forEach(item -> listC.add(item));//forEach(item -> listC.add(item)另一写法为forEach (listC::add)

        System.out.println(listC);
    }
}
```

## 注意
- Stream 就如同一个迭代器（Iterator），单向，不可往复，数据只能遍历一次，遍历过一次后即用尽了，就好比流水从面前流过，一去不复返。

## 生成流
在 Java 8 中, 集合接口有两个方法来生成流：
- stream() − 为集合创建串行流。
- parallelStream() − 为集合创建并行流。

## Stream流的构造 及 转换

``` Java
public class Demo{
  public static void main(String [] args){
    //转为Stream对象  
    //  数组转Stream对象
    String[] stringArray = new String[]{"a", "b", "c"};
    //    使用Stream的of()
    Stream stream1 = Stream.of(stringArray);//Stream 的of()调用的还是Arrays.stream()
    //    使用数组工具类Arrays的stream()
    Stream stream2 = Arrays.stream(stringArray);
    //  集合转Stream对象
    List<String> list = Arrays.asList(stringArray);
    //    使用List接口的stream()
    Stream stream3 = list.stream();

    //Stream转为Array
    String[] strArray = (String[]) stream1.toArray(String[]::new);
    //Stream转为Collection
    List<String> list1 = (List<String>) stream3.collect(Collectors.toList());
  }
}
```

## Stream操作的特征
- Pipelining: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
- 内部迭代： 以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。

## Stream的常用操作
- Intermediate：(中间操作)
  - 一个流可以后面跟随零个或多个 intermediate 操作。其目的主要是打开流，做出某种程度的数据映射/过滤，然后返回一个新的流，交给下一个操作使用。
  - <font color='red'>这类操作都是惰性化的（lazy），就是说，仅仅调用到这类方法，并没有真正开始流的遍历</font>
    - 包含的操作：map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered 

- Terminal：(终点操作)
  - 一个流只能有一个 terminal 操作，当这个操作执行后，流就被使用“光”了，无法再被操作。所以这必定是流的最后一个操作。  
  - <font color='red'>Terminal 操作的执行，才会真正开始流的遍历，并且会生成一个结果，或者一个 side effect</font>
    - 包含的操作有：forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator

## Stream操作案例
``` Java
public class Demo{
  public static void main(String [] args){
    //  forEach 来迭代流中的每个数据
    Random random = new Random();
    random.ints().limit(10).forEach(System.out::println);

    //  map 方法用于映射每个元素到对应的结果
    List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
    List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());// 获取对应的平方数

    //  filter 方法用于通过设置的条件过滤出元素
    List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
    long count = strings.stream().filter(string -> string.isEmpty()).count();// 获取空字符串的数量

    //  limit 方法用于获取指定数量的流
    Random random = new Random();
    random.ints().limit(10).forEach(System.out::println);

    //  parallelStream 是流并行处理程序的代替方法
    List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
    long count = strings.parallelStream().filter(string -> string.isEmpty()).count();//// 获取空字符串的数量
  }
}
```

## for、foreach、stream 哪家的效率更高？
1万以内的数据，for循环的性能要高于foreach和stream；
数据量上去之后（1000万），三种遍历方式基本已经没有什么差距了，但是Stream提供并行处理，在数据量大了之后，效率会明显增强。（但是单核CPU，Stream并行处理可能会效率更慢）