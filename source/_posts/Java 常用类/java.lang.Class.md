---
title: Class
date: 2021-12-22 10:41:43
tags:
  - Java常用类
categories:
  - Java
---

# Class

## 概述
- java有两种对象: 实例对象和Class对象
- Class对象: 表示正在运行的 类和接口 的`运行时的类型信息`,包含了与类和接口有关的信息
- 实例对象: 实例对象是通过Class对象来创建的
- `枚举是一种类，注释是一种接口`
- 每一个类都有一个Class对象，每当编译一个新类就产生一个Class对象
- Class对象对应着`java.lang.Class`类，如果说类是对象抽象和集合的话，那么Class类就是对类的抽象和集合
- Class类没有公共的构造方法，Class对象是在类加载的时候由Java虚拟机以及通过调用类加载器中的 defineClass 方法自动构造的，因此不能显式地声明一个Class对象
- Class类继承自Object

## 类被加载到内存需要经历如下三个阶段：
- 加载，这是由类加载器（ClassLoader）执行的;`生成一个代表这个类的java.lang.Class对象`
通过一个类的全限定名来获取其定义的二进制字节流（Class字节码），将这个字节流所代表的静态存储结构转化为方法去的运行时数据接口，根据字节码在java堆中生成一个代表这个类的java.lang.Class对象。
- 链接;`为静态域分配存储空间并设置类变量的初始值`
在链接阶段将验证Class文件中的字节流包含的信息是否符合当前虚拟机的要求，为静态域分配存储空间并设置类变量的初始值（默认的零值），并且如果必需的话，将常量池中的符号引用转化为直接引用。
- 初始化;`真正开始执行类中定义的java程序代码`
到了此阶段，才真正开始执行类中定义的java程序代码。用于执行该类的静态初始器和静态初始块，如果该类有父类的话，则优先对其父类进行初始化。

## 类的加载方式
- 所有的类都是在对其第一次使用时，动态加载到JVM中的（懒加载）。当程序创建第一个对类的静态成员的引用时，就会加载这个类。使用new创建类对象的时候也会被当作对类的静态成员的引用。动态加载:`java程序程序在它开始运行之前并非被完全加载，其各个类都是在必需时才加载的`
- 在类加载阶段，类加载器首先检查这个类的Class对象是否已经被加载。如果尚未加载，默认的类加载器就会根据类的全限定名查找.class文件。在这个类的字节码被加载时，它们会接受验证，以确保其没有被破坏，并且不包含不良java代码。一旦某个类的Class对象被载入内存，我们就可以它来创建这个类的所有对象

## 获得Class对象的方式：
- Class.forName(“类的全限定名”) `类的全限定名:类全名,包括包名`
  - `Class.forName()方法是Class类的一个静态成员`
  - `forName在执行的过程中如果发现类XXX还没有被加载，那么JVM就会调用类加载器去加载XXX类，并返回加载后的Class对象`如果Class .forName找不到你要加载的类，它会抛出`ClassNotFoundException`异常
  - 不需要为了获得Class引用而持有该类型的对象，只要通过全限定名就可以返回该类型的一个Class引用,对Class引用立即进行了初始化
- 实例对象.getClass()
  - 如果已经有了该类型的对象，可以通过调用getClass()方法来获取Class引用，这个方法属于根类Object的一部分，它返回的是表示该对象的实际类型的Class引用
  - 利用new操作符创建对象后，类已经装载到内存中了，所以执行getClass()方法的时候，就不会再去执行类加载的操作了，而是直接从java堆中返回该类型的Class引用
- 类名.class （类字面常量）
  - `类名.class`这样做不仅更简单，而且更安全，因为它在编译时就会受到检查
  - 根除了对forName()方法的调用，所以也更高效
  - 类字面量不仅可以应用于普通的类，也可以应用于接口、数组及基本数据类型
  - 用.class来创建对Class对象的引用时，不会自动地初始化该Class对象（这点和Class.forName方法不同）;类对象的初始化阶段被延迟到了对静态方法或者非常数静态域首次引用时才执行

## 获取Class对象案例
``` Java
public class Test1 {
    //  静态代码块
    static {
        System.out.println("Loading Test1");
    }

    public static void main(String[] args) {
        try {
            //  使用Class.forName("类的全名,包括包名")获取Class对象
            Class c = Class.forName("Test1");
            System.out.println(c);
        } catch (ClassNotFoundException e) {
            System.out.println("Couldn't find Test1");
        }

        //  使用实例对象.getClass()获取Class对象
        Test1 test1 = new Test1();
        Class c = test1.getClass();
        System.out.println(c);

        //  使用类.class获取Class对象
        System.out.println(Test1.class);

    }
}
```

## 注意
- 基本数据类型的Class对象和包装类的Class对象是不一样的
- 在包装类中有个字段TYPE，TYPE字段是一个引用，指向对应的基本数据类型的Class对象

## 编译时常量
如果一个字段被static final修饰，我们称为`编译时常量`;那么在调用这个字段的时候是不会对当前类进行初始化的

## Class对象
- 类被加载到内存中，那么不论通过哪种方式获得该类的Class对象，它们返回的都是指向同一个java堆地址上的Class引用。jvm不会创建两个相同类型的Class对象
- 其实对于任意一个Class对象，都需要由它的`类加载器`和`这个类本身`一同确定其在Java虚拟机中的唯一性
- `即使两个Class对象来源于同一个Class文件，只要加载它们的类加载器不同，那这两个Class对象就必定不相等`这里的“相等”包括了代表类的Class对象的equals（）、isAssignableFrom（）、isInstance（）等方法的返回结果，也包括了使用instanceof关键字对对象所属关系的判定结果。所以在java虚拟机中使用双亲委派模型来组织类加载器之间的关系，来保证Class对象的唯一性。

## 泛型Class引用
- Class引用表示的就是它所指向的对象的确切类型，而该对象便是Class类的一个对象。
- 在JavaSE5中，允许你对Class引用所指向的Class对象的类型进行限定，也就是说你可以对Class对象使用泛型语法。通过泛型语法，可以让编译器强制指向额外的类型检查

## Class类的方法
- static Class<?> forName()	
  - 获取Class对象的引用，引用的类还没有加载就加载这个类
  - 为了产生Class引用，forName()立即就进行了初始化
- getClass()	
  - 获取Class对象的引用，返回表示该对象的实际类型的Class引用;该方法继承自Object
- getName()	
  - 取全限定的类名(包括包名)，即类的完整名字
- getClassLoader() 
  - 返回该类的类加载器
- newInstance()	
  - 返回一个Oject对象，是实现“虚拟构造器”的一种途径。使用该方法创建的类，必须带有无参的构造器
- getFields()	
  - 获得某个类的所有的公共（public）的字段，包括继承自父类的所有公共字段。 类似的还有getMethods和getConstructors
- getDeclaredFields	
  - 获得某个类的自己声明的字段，即包括public、private和proteced，默认但是不包括父类声明的任何字段。类似的还有getDeclaredMethods和getDeclaredConstructors。



