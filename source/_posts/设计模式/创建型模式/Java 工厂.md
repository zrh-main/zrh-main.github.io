---
title: 工厂模式
tags:
  - 创建型模式
categories: 设计模式
date: 2021-12-22 14:53:58
---

## 概述
工厂方法即Factory Method，是一种对象创建型模式

## 目的(作用)
让创建对象和使用对象分离，并且调用方总是引用抽象工厂和抽象产品

``` Java
工厂接口
//  定义一个用于创建对象的接口
public interface NumberFactory {
    Number parse(String s);
    static NumberFactory impl = new NumberFactoryImpl();
    //  获取工厂实例:
    //  定义一个静态方法getFactory()来返回真正的子类
    static NumberFactory getFactory() {
        return impl;
    }
    
}
工厂的实现类
public class NumberFactoryImpl implements NumberFactory {
    public Number parse(String s) {
        return new BigDecimal(s);
    }
}
调用方使用
NumberFactory factory = NumberFactory.getFactory();
Number result = factory.parse("123.456");
```
调用方可以完全忽略真正的工厂NumberFactoryImpl和实际的产品BigDecimal，这样做的好处是允许创建产品的代码独立地变换，而不会影响到调用方


## 静态工厂

通过静态方法直接返回产品
``` Java
public class NumberFactory{
    public static Number parse(String s){
        return new BigDecimal(s);
    }
}
```
`这种简化的使用静态方法创建产品的方式称为静态工厂方法（Static Factory Method）`
静态工厂方法广泛地应用在Java标准库中
## 优势
 工厂方法可以隐藏创建产品的细节，且不一定每次都会真正创建产品，完全可以返回缓存的产品，从而提升速度并减少内存消耗


## 提示
** 总是引用接口而非实现类，能允许变换子类而不影响调用方，即尽可能面向抽象编程**

## 总结
工厂方法是指定义工厂接口和产品接口，但如何创建实际工厂和实际产品被推迟到子类实现，从而使调用方只和抽象工厂与抽象产品打交道。

实际更常用的是更简单的静态工厂方法，它允许工厂内部对创建产品进行优化。

调用方尽量持有接口或抽象类，避免持有具体类型的子类，以便工厂方法能随时切换不同的子类返回，却不影响调用方代码