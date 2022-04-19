---
title: Java面试题
tags:
  - Java面试题
categories:
  面试题
---

## JavaSE面试题
1. jdk，jre，jvm的含义以及三者之间的关系?
  JVM: Java虚拟机，是Java程序的运行环境,
  JRE: Java程序的运行时环境,包含JVM和运行时需要的核心类库
  JDK: Java程序的开发工具包,包含JRE和开发使用的工具
2. i++和++i的区别?
  i++ 先执行,后自增
  ++i 先自增,后执行
3. short s = 1,s = s + 1 有错吗？为什么？s += 1有错吗？为什么？

4. ArrayIndexOutOfBoundsException（数组下标越界异常）和NullPointerException(空指针异常)出现的原因是?


7. 值传递和址传递的区别?

8. 成员变量,局部变量,静态变量之间的区别?

9. 成员方法与静态方法的区别?

10. 包装类的来历?



12. 泛型的含义及使用




19. super与this代表的含义

20. 面向对象中,创建子类对象会创建父类对象吗?画出内存中父子类的关系形式

21. 匿名内部类的使用场景及编写使用方式

22. ==和equals的区别,以及它们之间的关系

22. 获取当前系统时间的方式,写出2-3种



24. String[] strArr = {"a", "b", "c", "d", "e"} 转换为集合,使用迭代器遍历,删除其中的 c 字符串,最后打印删除 c 字符串后的集合

25. 什么情况下会出现ClassCastException异常

26. Java中的泛型是什么 ? 使用泛型的好处是什么?

27. Java的泛型是如何工作的 ? 什么是类型擦除 ?

28. 什么是泛型中的限定通配符和非限定通配符 ?

30. 如何编写一个泛型方法，让它能接受泛型参数并返回泛型类型?

32. 编写一段泛型程序来实现LRU缓存?

33. 你可以把List<String>传递给一个接受List<Object>参数的方法吗？

34. Java中创建对象的方式有几种?何时使用哪种方式?
    - 使用
      1. 使用new关键字
      2. 使用clone方法
      3. 反射机制
      4. 反序列化
    - 区别
        1,3都会明确的显式的调用构造函数
        2是在内存上对已有对象的拷贝 所以不会调用构造函数
        4是从文件中还原类的对象 也不会调用构造函数
    
35. 说一下浅拷贝与深拷贝



37. 什么是哈希冲突



39. HashSet集合与Map的关系
  - HashSet类中有一个全局变量HashMap map,然后在HashSet的构造函数中有一句话map=new HashMap(),说明在创建HashSet类对象的时候底层创建了一个HashMap对象

40. hashCode的本质
  - 帮助HashMap和HashSet集合加快插入的效率，当插入一个数据时，通过hashCode能够快速地计算插入位置，就不需要从头到尾地使用equlas方法进行比较

41. 自定义类型的实例对象使用set存储或作为Map的键需要做什么?
  - 因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的对象必须重写这两个方法
  - 如果自定义对象作为 Map 的键，那么必须重写 hashCode 和 equals

42. 为什么重写equals
  - Object类的equals，比较的是对象的地址值
  - 如果希望比较对象的值是否相同，必须重写equals方法

43. 为什么重写equals就必须重写hashCode?
  1. equals()和hashCode()的作用
    - equals()  用来比较两个对象是否相等;没有重写比较两个对象的地址值(地址值不会相同),若是想让两个内容相同的对象在equals后得到true，则需重写equals方法
    - hashCode()  用来返回对象的hash码值,通常情况下，我们都不会使用到这个方法;本质是为了帮助HashMap和HashSet集合加快插入的效率
  2. 为什么只要重写了equals方法，就必须重写hashCode
    - 主要是针对一些使用到了hashCode方法的集合，比如HashMap、HashSet等
    - 当equals方法被重写时，两个对象的内容相同即代表相同;但是如果没有重写hashCode方法,向hash结构的集合中添加这两个对象时,两个都会添加成功,因为没有重写hascode，生成的是两个hash值，在hash集合看来就是两个对象;说明这里必须重写hashCode(),两个内容相同的对象hash值相同,hash结构才会认为是一个对象;

44. 哈希结构的集合中添加元素
  - 先调用hashCode，唯一则存储，不唯一则再调用equals，结果相同则不再存储，结果不同则散列到其他位置。因为hashCode效率更高（仅为一个int值），比较起来更快

45. Apache和Apache Tomcat的区别是什么？
  - Apache 和 Tomcat 都是web网络服务器，两者既有联系又有区别
  - Apache是web服务器（静态解析，如HTML），tomcat是java应用服务器（动态解析，如JSP）
  - Tomcat只是一个servlet(jsp也翻译成servlet)容器，可以认为是apache的扩展，但可以独立于apache运行
  - Apache和Tomcat是独立的，在同一台服务器上可以集成
  - Apache和Tomcat整合使用
      如果客户端请求的是静态页面，则只需要Apache服务器响应请求；
      如果客户端请求动态页面，则是Tomcat服务器响应请求，将解析的JSP等网页代码解析后回传给Apache服务器，再经Apache返回给浏览器端。
      这是因为jsp是服务器端解释代码的，Tomcat只做动态代码解析，Apache回传解析好的静态代码，Apache+Tomcat这样整合就可以减少Tomcat的服务开销。
  - APACHE+TOMCAT+JDK整合的好处：
      如果客户端请求的是静态页面，则只需要Apache服务器响应请求 如果客户端请求动态页面，则是Tomcat服务器响应请求
      因为jsp是服务器端解释代码的，这样整合就可以减少Tomcat的服务开销

46. Java 中 clone() 和 new 效率哪个更高？

拷贝对象返回的是一个新的对象，而不是一个对象的引用地址；
拷贝对象已经包含原来对象的信息，而不是对象的初始信息，即每次拷贝动作不是针对一个全新对象的创建。

利用clone，在内存中进行数据块的拷贝，复制已有的对象，也是生成对象的一种方式。前提是类实现Cloneable接口，Cloneable接口没有任何方法，是一个空接口，也可以称这样的接口为标志接口，只有实现了该接口，才会支持clone操作。有的人也许会问了，java中的对象都有一个默认的父类Object。

Object中有一个clone方法，为什么还必须要实现Cloneable接口呢，这就是cloneable接口这个标志接口的意义，只有实现了这个接口才能实现复制操作，因为jvm在复制对象的时候，会检查对象的类是否实现了Cloneable这个接口，如果没有实现，则会报CloneNotSupportedException异常。类似这样的接口还有Serializable接口、RandomAccess接口等。

还有值得一提的是在执行clone操作的时候，不会调用构造函数。还有clone操作还会面临深拷贝和浅拷贝的问题。关于这方面的问题，网上有很多的相关知识了，不再累述了。由于通过复制操作得到对象不需要调用构造函数，只是内存中的数据块的拷贝，那是不是拷贝对象的效率是不是一定会比new的时候的快。

答案：不是。显然jvm的开发者也意识到通过new方式来生成对象占据了开发者生成对象的绝大部分，所以对于利用new操作生成对象进行了优化。

结论：轻量级的对象可以使用new，其他对象可以使用clone。


46. DataSource 和DriverManager的区别
  1. 获取的对象不同。DataSource主要是获取数据库连接池，而DriverManager主要是获取数据库连接，通过管理JDBC驱动程序来建立连接。
  2. DataSource中封装了DriverManager的使用
  3. DataSource创建的connection可以被复用，而DriverManager的则不行

47. TCP与UDP的区别;TCP协议的三次握手,四次挥手

48. 网络通信的三要素

49. final和finally的区别
  - final是一个安全修饰符,用final修饰的类不能被继承,用final声明的方法不能被重写,使用final声明的变量是常量,不能被修改
  - finally是在异常里经常用到的, 是try和cach里的代码执行完以后,必须要执行的方法,主要执行一些释放资源的操作,比如说关闭数据库连接,或者关闭IO流


49. int和Integer的区别？
  - int是基本数据类型;Integer是java为int提供的封装类，是引用数据类型；
  - int的默认值为0，而integer的默认值为null

49. servlet的生命周期？
  - init：初始化方法，默认在第一次访问时被执行，只执行1次
  - service：提供服务的方法，在每次访问时都会执行，执行多次
  - destory：销毁的方法。会在服务器正常关闭的之后执行1次



## String相关
1. String类中常用的方法
  - split         将字符串拆分成字符串数组
  - indexOf       获取指定字符或字符串在当前字符串中首次出现的位置,没找到返回-1
  - substring     截取字符串,并返回新的字符串
  - replace()：   替换字符串内匹配的子字符串
  - equals()：    比较字符串的内容是否相同
  - concat()：    将指定字符串连接到此字符串的结尾
2. String中的==和equals的区别
  - == 默认比较的是地址值是否相同
  - equals默认比较的也是地址值是否相同,但String中的equals进行了重写,比较的是内容是否相同
3. Java中的String，StringBuilder，StringBuffer三者的区别
  - String是长度不可变的字符序列;一旦创建，内容不能改变;主要用于少量的字符串操作
  - StringBuilder类是一个长度可变的字符序列;线程不安全，效率高;append方法拼接内容,不会创建新的对象
  - StringBuffer类是一个长度可变的字符序列;线程安全，效率低;append方法拼接内容,不会创建新的对象

4. String s = “hello” 和 String s = new String(“hello”)的区别?

5. String s1 = “hello”,s1 += “word”, 问原始的s1的内容改变了 吗？


## Collecation集合相关
1. java arrayList的存储结构,初始化的时候创建多大的数组?
  - ArrayList是基于数组实现的，是一个动态数组,容量能自动增长，初始化长度是10, 扩容规则:  扩容后的大小= 原始大小*1.5
  - ArrayList是线程不安全的，只能用在单线程环境下，多线程环境下可以考虑用`Collections.synchronizedList(List l)`函数返回一个线程安全的ArrayList类，也可以使用concurrent并发包下的`CopyOnWriteArrayList`类
2. ArrayList与LinkedList区别及使用
  - 数据结构不同;ArrayList是Array(动态数组)的数据结构，LinkedList是Link(链表)的数据结构
  - 效率不同
    - 查询时
      LinkedList是线性的数据存储方式，需要移动指针从前往后依次查找
      ArrayList使用数组方式存储数据，根据索引查询数据速度快
    - 新增或删除时
      LinkedList使用链表结构存储数据，每个元素都记录前后元素的指针，所以插入、删除数据时只是更改前后元素的指针指向即可,操作比较快
      ArrayList使用数组存储数据，所以在其中进行增删操作时，新增或删除后会对所有数据的下标索引造成影响，需要进行数据的移动,操作比较慢
  - ArrayList使用数组方式存储数据，根据索引查询数据速度快，而新增或者删除元素时需要位移操作，所以比较慢。 
	- LinkedList使用双向链接方式存储数据，每个元素都记录前后元素的指针，所以插入、删除数据时只是更改前后元素的指针指向即可，速度非常快，通过下标查询元素时需要从头开始索引，所以比较慢，但是如果查询前几个元素或后几个元素速度比较快。开发中什么时候到ArrayList?,我们在做查询的时候把查询出来的数据经常存到arraylist里.
3. Collection和Collections的区别？
4. List集合与Set集合的特性
5. 集合与数组的区别?
11. ArrayList和linkedList作用及区别,分别介绍常用的方法?

## 面向对象相关
49. Java里可不可以有多继承？
  - 不可以，想多继承的话,使用接口
15. 什么是类？什么是对象？

16. 说一下面向对象三大特征：封装，继承，多态
  - 继承就是子类继承父类的属性和方法(使用extends)
  - 封装,使用 private 把成员变量设置为私有，然后可以对外提供 public 的 set 和 get 方法
  - 多态,多态就是同一个类或接口，使用不同的实例而执行不同操作

17. 重载与重写的区别,作用?
  - 区别
    重写是父类与子类之间的多态;
    重载是在一个类中多态的体现;
  - 作用
    方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)
    方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)
18. 抽象类与接口的区别,包括使用(什么情况使用抽象类,什么情况使用接口)
## 内存相关
1. 内存概述
  内存是计算机的临时存储设备，当程序被启动时，会被cpu加载到内存中。
2. 内存区域划分
  寄存器：计算机直接使用，我们不需要管
  本地方法栈：jvm使用操作系统功能时使用，不需要管。
  栈区：存放局部变量，基本数据类型等。
    堆区：存放引用数据类型。
  方法区：存放运行时的字节码文件。