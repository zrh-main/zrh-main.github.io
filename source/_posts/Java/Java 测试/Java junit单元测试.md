---
title: Java junit单元测试
date: 2022-2-11 14:56:43
tags:
  - Java 测试
categories: Java
---

## 概述

## 分类
  - 黑盒测试：不需要写代码，输入值，看程序是否能够输出期望的值`(关注结果)`
  - 白盒测试：需要写代码;关注程序具体的执行流程`(关注过程)`
  - **这里关注白盒测试...**

## 依赖
  - junit-版本号&nbsp;&nbsp;&nbsp;jar包
  - hamcrest-core-版本号&nbsp;&nbsp;&nbsp;jar包`(这里是junit jar包的依赖)`

## 作用
可以让任意方法都能运行

## 步骤
  1. 导入junit的jar包
  2. 创建测试类
  3. 给测试方法加上`@Test`注解
  4. 运行方法

## 建议
  - 方法名：test测试的方法名		testAdd()  
  - 返回值：void
  - 参数列表：空参

## 断言
一般情况下，测试方法中不会看输出，只会看运行后的颜色
使用`Assert.assertEquals(期望的结果,运算的结果)`来判断结果的颜色
  - 红色：`失败`
  - 绿色：`成功`

## 补充
@Before:
如果在测试类中一个方法加上此注解，那么该方法会在测试方法运行前运行。
（一般会用于在测试类中加载一些初始化资源）
@After:
如果在测试类中一个方法加上此注解，那么该方法会在测试方法运行后运行。
（一般会用于释放资源）

## 案例
需要测试的类
``` Java
public class Result {
    /**
     * 加
     */
    public int sum(int a,int b){
        return a + b;
    }
    /**
     * 减
     */
    public int minus(int a,int b){
        return a - b;
    }
}

```
测试类
``` Java
public class Test2 {
    @Before
    public void testBefore(){
        System.out.println("初始化资源....");
    }

    @After
    public void testAfter(){
        System.out.println("释放资源......");
    }

    @Test
    public void testSum(){
        Result result = new Result();
        int i = result.sum(3,2)
        Assert.assertEquals(5,i);
    }

    @Test
    public void testMinus(){
        Result result = new Result();
        int i = result.minus(3,5)
        Assert.assertEquals(-2,i);
    }
}
```