---
title: 注解
date: 2021-12-09 10:58:39
tags:
  - Java SE基础语法
categories: Java
---

## 概述
Java 注解(Annotation)又称`Java 标注`，是`JDK5.0 引入`的一种注释机制(一种特殊的接口)
注解本质上就是一个接口，该接口默认继承`Annotation`接口
Java 语言中的类、方法、变量、参数和包等都可以被标注。Java 标注可以通过反射获取标注内容
注解: 对程序进行说明解释; 给计算机看的
注释：对程序进行说明解释; 给程序员看的
## 内置注解

### 作用在代码的注解
@Override - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
@Deprecated - 标记过时方法。如果使用该方法，会报编译警告。
@SuppressWarnings - 指示编译器去忽略注解中声明的警告。

### 元注解(作用在其他注解的注解)是:
元注解：描述注解的注解
@Retention - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。
@Documented - 标记这些注解是否包含在用户文档中。
@Target - 标记这个注解应该是哪种 Java 成员。
@Inherited - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)

### 新增注解
@SafeVarargs - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
@FunctionalInterface - Java 8 开始支持，标识一个匿名函数或函数式接口。
@Repeatable - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。

## Annotation 架构
![](Annotation架构.jpg)

## Annotation 组成部分
java Annotation 的组成中，有 3 个非常重要的主干类。它们分别是：

**Annotation.java**
Annotation 是个接口
``` Java
public interface Annotation {
    boolean equals(Object obj);
    int hashCode();
    String toString();
    Class<? extends Annotation> annotationType();
}
每个 Annotation 对象，都会有唯一的 RetentionPolicy 属性和 1~n 个ElementType 属性
```

**ElementType.java**
ElementType 是 Enum 枚举类型，它用来指定 Annotation 的类型
``` Java
package java.lang.annotation;
public enum ElementType {
  TYPE,               /* 类、接口（包括注释类型）或枚举声明  */
  FIELD,              /* 字段声明（包括枚举常量）  */
  METHOD,             /* 方法声明  */
  PARAMETER,          /* 参数声明  */
  CONSTRUCTOR,        /* 构造方法声明  */
  LOCAL_VARIABLE,     /* 局部变量声明  */
  ANNOTATION_TYPE,    /* 注释类型声明  */
  PACKAGE             /* 包声明  */
}
```

**RetentionPolicy.java**
RetentionPolicy 是 Enum 枚举类型，它用来指定 Annotation 的策略;
就是不同 RetentionPolicy 类型的 Annotation 的作用域不同
``` Java
package java.lang.annotation;
public enum RetentionPolicy {
    SOURCE,            /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */
    CLASS,             /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */
    RU
    NTIME            /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
SOURCE ：Annotation 仅存在于编译器处理期间，编译器处理完之后，该 Annotation 就没用了
CLASS ：编译器将 Annotation 存储于类对应的 .class 文件中，它是 Annotation 的默认行为
RUNTIME ：编译器将 Annotation 存储于 class 文件中，并且可由JVM读入
```
## 自定义注解

Annotation 通用定义
``` Java
@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation1 {
}

```
说明：
定义一个 Annotation,名字是 MyAnnotation1,在代码中通过 "@MyAnnotation1" 来使用它
其余注解用来修饰MyAnnotation1
1. @interface
    意味着MyAnnotation1实现了 java.lang.annotation.Annotation 接口，即表示这是一个Annotation`(这是一个注解)`
`定义 Annotation 时，@interface 是必须的`
注意：`它和我们通常的 implemented 实现接口的方法不同。Annotation 接口的实现细节都由编译器完成。通过 @interface 定义注解后，该注解不能继承其他的注解或接口`
2. @Documented
类和方法的 Annotation 在缺省情况下是不出现在 javadoc 中的
如果使用 @Documented 修饰该 Annotation，则表示它可以出现在 javadoc 中。
定义 Annotation 时，@Documented 可有可无；若没有定义，则 Annotation 不会出现在 javadoc 中
3. @Target(ElementType.TYPE)
ElementType 是 Annotation 的类型属性。而 @Target 的作用，就是来指定 Annotation 的类型属性
@Target(ElementType.TYPE) 的意思就是指定该 Annotation 的类型是 ElementType.TYPE
这就意味着，MyAnnotation1 是来修饰"类、接口（包括注释类型）或枚举声明"的注解
定义 Annotation 时，@Target 可有可无。若有 @Target，则该 Annotation 只能用于它所指定的地方；若没有 @Target，则该 Annotation 可以用于任何地方
4. @Retention(RetentionPolicy.RUNTIME)
RetentionPolicy 是 Annotation 的策略属性，而 @Retention 的作用，就是指定 Annotation 的策略属性`(运行环境)`
@Retention(RetentionPolicy.RUNTIME) 的意思就是指定该 Annotation 的策略是 RetentionPolicy.RUNTIME
这就意味着，编译器会将该 Annotation 信息保留在 .class 文件中，并且能被虚拟机读取
定义 Annotation 时，@Retention 可有可无。若没有 @Retention，则默认是 RetentionPolicy.CLASS。

## 常用注解(常用的 Annotation)

@Deprecated  -- @Deprecated 所标注内容，不再被建议使用。
@Override    -- @Override 只能标注方法，表示该方法覆盖父类中的方法。
@Documented  -- @Documented 所标注内容，可以出现在javadoc中。
@Inherited   -- @Inherited只能被用来标注“Annotation类型”，它所标注的Annotation具有继承性。
@Retention   -- @Retention只能被用来标注“Annotation类型”，而且它被用来指定Annotation的RetentionPolicy属性。
@Target      -- @Target只能被用来标注“Annotation类型”，而且它被用来指定Annotation的ElementType属性。
@SuppressWarnings -- @SuppressWarnings 所标注内容产生的警告，编译器会对这些警告保持静默。
由于 "@Deprecated 和 @Override" 类似，"@Documented, @Inherited, @Retention, @Target" 类似；下面，我们只对 @Deprecated, @Inherited, @SuppressWarnings 这 3 个 Annotation 进行说明。

## Annotation 的作用
Annotation 是一个辅助类，它在 Junit、Struts、Spring 等工具框架中被广泛使用。
用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能
1. 编译检查
Annotation 具有"让编译器进行编译检查的作用"。
例如，@SuppressWarnings, @Deprecated 和 @Override 都具有编译检查作用。

2. 在反射中使用 Annotation
在反射的 Class, Method, Field 等函数中，有许多于 Annotation 相关的接口。

3. 根据 Annotation 生成帮助文档
通过给 Annotation 注解加上 @Documented 标签，能使该 Annotation 标签出现在 javadoc 中。

4. 能够帮忙查看查看代码
通过 @Override, @Deprecated 等，我们能很方便的了解程序的大致结构。

5. 可以通过自定义 Annotation 来实现一些功能(自定义注解)
 
## 注解中的属性
属性：接口中的抽象方法
*要求：
1.属性的返回值类型有下列取值：
* 基本数据类型
* String
* 枚举
* 注解
* 以上类型的数组
2.定义了属性，在使用时必须要给属性赋值
1. 如果定义属性时，使用default关键字给属性默认初始化值，则使		用注解时，可以不进行属性的赋值。
2. 如果只有一个属性需要赋值，并且属性的名称是value，则value		可以省略，直接定义值即可。
3. 数组赋值时，值使用{}包裹。如果数组中只有一个值，则{}可以省		略

### 属性案例
定义注解
``` Java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target({ElementType.METHOD,ElementType.TYPE,ElementType.FIELD})
public @interface AA{
    String[] name() default "张三";
}
```
使用注解
``` Java
@AA(name= "呵呵")
public class Test1 {
    @AA(name= "呵呵")
    String name;

    @AA(name= "呵呵")
    public static void main(String[] args) {
        System.out.println("aa");
    }
}
```
