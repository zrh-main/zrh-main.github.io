---
title: Java 关键字
tags:
  - Java重点
categories:
  Java
---
## this && super

## this
  - this表示一个对象的引用,它指向正在执行的对象. 
  - 在构造方法中,通过this关键字调用其他构造方法时,必须放在第一行, 且在构造方法中, 只能通过this调用一次其他构造方法

#### 使用
  - 调用当前对象的属性: this.属性名
  - 调用当前对象的方法: this.方法名

## super
  - 创建子类对象的时候，会调用子类的构造方法，子类构造方法执行之前，会默认访问父类的无参构造。

#### 使用
  - 调用父类的无参构造:   super()
  - 调用父类的带参构造:   super(参数)
  - 调用父类的成员变量:   super.变量名;	
  - 调用父类的方法:       super.方法名();

### this && super  !注意事项

  - super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。
  - this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。
  - this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
  - this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。

## extends

用来表达类与类之间的继承关系;类之类只可多重继承,不可多次继承
``` Java
public class A extends B{}
```

用来表达接口与接口之间的继承关系;接口与接口可以多重继承,也可以多次继承
``` Java
public interface 子接口名称 extends 父接口名称1,父接口名称2{}
```

## abstract
用来声明抽象类和抽象方法
``` Java
public abstract 类名称{
  public abstract 抽象方法名称();
}
```

## interface 
用来声明接口
``` Java
// 声明方式
public interface 接口名称{}
```

## implements
类用来实现接口
``` Java
// 单实现
public class 类名称 implements 接口名称{}
// 多实现
public class 类名称 implements 接口名称1,接口名称2{}
```

## static 状态修饰符

## private 访问修饰符

## public 访问修饰符

## default 访问修饰符

## instanceof 
判断对象的类型
