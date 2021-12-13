---
title: Java 循环性能
tags:
  - Java重点
categories:
  Java
---

多种循环方式，迭代器、for循环、forEach循环、lambda的forEach循环之间的性能问题...
1. ArrayList
 当size很大（10万）时候for循环的性能最高，当size不是很大的时候Iterator和forEach效率就高
 如何选择可参考
  - 如果是需要对下标进行处理操作对则选择for循环；
  - 对于ArrayList，从代码简洁讲优先考虑foreach，也不用去考虑下标越界对问题；
  - 对于实效性要求不高的，如I/O操作，可考虑lambda的forEach循环。

2. LinkedList
 此时Iterator性能最佳，由于LinkedList的数据结构为链表结构，其中每个节点都记录了前驱和后继结点（头、尾除外）。
 通过下标的方式获得值都会在链中检查一遍，直到得到我们想要的下标节点为止。
 而Iterator和forEach则是始终循环子节点，直到尾节点为止。考虑到代码简洁，建议使用forEach。

3. HashMap
Iterator循环始终效率方面要占优，这里简单的提下HashMap的放值和取值原理。

   HashMap是一个散列桶，由数组和链表组成。每次put操作都会得到key的hash值，然后根据得到的hash值选择map数组对应的下标存入bucket（存入键对象和值对象）。而get操作时是根据键对象的hash值找map数组中对应的bucket，然后得到对应的值（当存在hash碰撞时，会以链表的形式加在相同hash值的后面，每次get的时候调用keys.equals（）得到该键与之对应的值）。

   在之前的List对比中已经说明forEach循环底层也是由Iterator实现，而lambda的forEach循环是充分利用cup实现多线程的迭代。所以不难得出HashMap遍历Iterator效率更高一点，但是基于代码简洁性考虑也可采用forEach循环方式。