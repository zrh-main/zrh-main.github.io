---
title: 关键字
tags:
  - Java SE基础语法
categories: Java
---
## this
概述:
  1. this表示一个对象的引用,它指向正在执行的对象. 
  2. 在构造方法中,通过this关键字调用其他构造方法时,必须放在第一行, 且在构造方法中, 只能通过this调用一次其他构造方法

使用
  1. 调用当前对象的属性: this.属性名
  2. 调用当前对象的方法: this.方法名

***

## super
概述:
  创建子类对象的时候，会调用子类的构造方法，子类构造方法执行之前，会默认访问父类的无参构造。

使用
  - 调用父类的无参构造:   super()
  - 调用父类的带参构造:   super(参数)
  - 调用父类的成员变量:   super.变量名;	
  - 调用父类的方法:       super.方法名();

***

## this && super  !注意事项

  - super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。
  - this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。
  - this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
  - this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。

***

## extends

用来表达类与类之间的继承关系;类之类只可多重继承,不可多次继承
``` Java
public class A extends B{}
```

用来表达接口与接口之间的继承关系;接口与接口可以多重继承,也可以多次继承
``` Java
public interface 子接口名称 extends 父接口名称1,父接口名称2{}
```

***

## interface 
用来声明接口
``` Java
// 声明方式
public interface 接口名称{}
```

***

## implements
类用来实现接口
``` Java
// 单实现
public class 类名称 implements 接口名称{}
// 多实现
public class 类名称 implements 接口名称1,接口名称2{}
```

***

## instanceof 
判断对象的类型

***

## 修饰符
修饰符用来定义类、方法或者变量，通常放在语句的最前端。
### 访问修饰符(权限修饰符)

#### 权限控制表
- YES:可以访问
- NO:不可以访问

|  修饰符    | 当前类 | 同一包内 | 子孙类(同一包) | 子孙类(不同包) | 其他包 |
| --------- | ------ | ------  | ------------- | -------  | ----------  |
| public    |  YES   |   YES   |       YES     |   YES    |     YES     |
| protected |  YES   |   YES   |       YES     |  YES/NO  |      NO     |
| default   |  YES   |   YES   |       YES     |    NO    |      NO     |
| private   |  YES   |   NO    |       NO      |    NO    |      NO     |

#### public 公共的

#### protected 受保护的(不能修饰外部类)
- 子类与基类在同一包中：被声明为 protected 的变量、方法和构造器能被同一个包中的任何其他类访问；
- 子类与基类不在同一包中：那么在子类中，子类实例可以访问其从基类继承而来的 protected 方法，而不能访问基类实例的protected方法。

#### default 默认的，什么修饰符都不加

#### private 私有的(不能修饰外部类)

### 非访问修饰符(状态修饰符)
#### static 
用来修饰静态成员;包含类方法(静态方法)和类变量(静态变量)
1. 概述:
    有static关键字的变量为静态变量;有static关键字的方法为静态方法,统称为静态成员
    静态变量不赋值默认值为null
    静态成员在内存的方法区
    静态成员随着类的加载而创建,随着类的销毁而销毁
- 调用静态变量:   类名.变量名
- 调用静态方法:   类名.方法名()
2. 注意:
    静态变量只有一份拷贝。局部变量不能被声明为 static 变量
    静态成员只能调用静态成员

#### final

1. 概述
    final是一个修饰符，作用是：最终的，不可改变的意思。

2. 作用
    修饰类，类为最终类，类不能被继承。
    修饰变量，变量为常量，只能被赋值1次，值不能改变。
    修饰方法，方法不能被重写

3. 注意
    final修饰成员变量，变量的值必须要给初始化，值不能改变。
    final修饰局部变量，变量的值允许赋值一次，值不能改变。
    final修饰基本类型，基本类型的值不能改变。
    final修饰引用类型，引用类型的地址值不能改变,内部数据可以改变
    final使用大写字母来表示

4. 例
    ``` Java
    //  被final修饰的类不能被继承
    public final class  Demo1 {
    //  final修饰基本类型，基本类型的值不能改变。
    //  final修饰引用类型，引用类型的地址值不能改变;内部数据可以改变
        
        //  final修饰成员变量
        public static final String CONSTANT = "String";
        public void test(){
            //  final修饰局部变量
            final int CONSTANT = 123456;
        }
    }
    ```
#### abstract
用来声明抽象类和抽象方法
``` Java
//抽象类
public abstract 类名称{
  //抽象方法
  public abstract 抽象方法名称();
}
//接口
public interface 接口名称{
  //抽象方法
  public abstract 抽象方法名称();
}
```

#### synchronized 和 volatile 修饰符，主要用于线程的编程

