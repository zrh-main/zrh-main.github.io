---
title: Java 代理
tags:
  - Java重点
categories: Java
date: 2021-12-22 14:53:58
---

# 代理

## 概述
代理模式（Proxy）是通过代理对象访问目标对象
![](代理模式.webp)
![](代理结构.webp)
## 作用
在目标对象基础上增强扩展额外的功能

## 关键
代理对象是对目标对象的扩展,并会调用目标对象

## 代理分类

### 静态代理
 - 需要定义父类或接口,目标对象与代理对象一起实现相同的接口或继承相同的父类,通过调用相同的方法来调用目标对象的方法
 - 缺点: 接口或父类增加方法,目标对象与代理对象都要维护,增加维护成本

#### 静态代理案例
AdminService.java接口
``` Java
public interface AdminService {
    void update();
    Object find();
}
```
AdminServiceImpl.java实现类
``` Java
public class AdminServiceImpl implements AdminService{
    public void update() {
        System.out.println("修改管理系统数据");
    }

    public Object find() {
        System.out.println("查看管理系统数据");
        return new Object();
    }
}
```
AdminServiceProxy.java代理类
``` Java
public class AdminServiceProxy implements AdminService {

    private AdminService adminService;

    public AdminServiceProxy(AdminService adminService) {
        this.adminService = adminService;
    }

    public void update() {
        System.out.println("判断用户是否有权限进行update操作");
        adminService.update();
        System.out.println("记录用户执行update操作的用户信息、更改内容和时间等");
    }

    public Object find() {
        System.out.println("判断用户是否有权限进行find操作");
        System.out.println("记录用户执行find操作的用户信息、查看内容和时间等");
        return adminService.find();
    }
}
```
测试类StaticProxyTest.java
``` Java
public class StaticProxyTest {
    public static void main(String[] args) {
        AdminService adminService = new AdminServiceImpl();
        AdminServiceProxy proxy = new AdminServiceProxy(adminService);
        proxy.update();
        System.out.println("=============================");
        proxy.find();
    }
}
```

### 动态代理(jdk代理,接口代理)
  - 特点: 
    1. 代理对象不需要实现接口或继承父类
    2. 代理对象的生成利用JDK的Api，在JVM内存中动态的构建Proxy对象。
    3. 需要使用`java.lang.reflect.Proxy`类的`static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,InvocationHandler invocationHandler)`
#### newProxyInstance(ClassLoader loader, Class<?>[] interfaces,InvocationHandler invocationHandler)详解
  - ClassLoader loader：指定当前目标对象使用类加载器，获取加载器的方法是固定的；
  - Class<?>[] interfaces：目标对象实现的接口的类型，使用泛型方式确认类型
  - InvocationHandler invocationHandler:事件处理,执行目标对象的方法时，会触发事件处理器的方法，会把当前执行目标对象的方法作为参数传入。

#### 动态代理案例
AdminServiceImpl.java和AdminService.java和原来一样

AdminServiceInvocation.java
``` Java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class AdminServiceInvocation  implements InvocationHandler {

    private Object target;

    public AdminServiceInvocation(Object target) {
        this.target = target;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("判断用户是否有权限进行操作");
       Object obj = method.invoke(target);
        System.out.println("记录用户执行操作的用户信息、更改内容和时间等");
        return obj;
    }
}
```
AdminServiceDynamicProxy.java
``` Java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;

public class AdminServiceDynamicProxy {
    private Object target;
    private InvocationHandler invocationHandler;
    public AdminServiceDynamicProxy(Object target,InvocationHandler invocationHandler){
        this.target = target;
        this.invocationHandler = invocationHandler;
    }

    public Object getPersonProxy() {
        Object obj = Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), invocationHandler);
        return obj;
    }
}
```
DynamicProxyTest.java
``` Java
package com.lance.proxy.demo.service;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class DynamicProxyTest {
    public static void main(String[] args) {

        // 方法一
        System.out.println("============ 方法一 ==============");
        AdminService adminService = new AdminServiceImpl();
        System.out.println("代理的目标对象：" + adminService.getClass());

        AdminServiceInvocation adminServiceInvocation = new AdminServiceInvocation(adminService);

        AdminService proxy = (AdminService) new AdminServiceDynamicProxy(adminService, adminServiceInvocation).getPersonProxy();

        System.out.println("代理对象：" + proxy.getClass());

        Object obj = proxy.find();
        System.out.println("find 返回对象：" + obj.getClass());
        System.out.println("----------------------------------");
        proxy.update();

        //方法二
        System.out.println("============ 方法二 ==============");
        AdminService target = new AdminServiceImpl();
        AdminServiceInvocation invocation = new AdminServiceInvocation(adminService);
        AdminService proxy2 = (AdminService) Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), invocation);

        Object obj2 = proxy2.find();
        System.out.println("find 返回对象：" + obj2.getClass());
        System.out.println("----------------------------------");
        proxy2.update();

        //方法三
        System.out.println("============ 方法三 ==============");
        final AdminService target3 = new AdminServiceImpl();
        AdminService proxy3 = (AdminService) Proxy.newProxyInstance(target3.getClass().getClassLoader(), target3.getClass().getInterfaces(), new InvocationHandler() {

            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("判断用户是否有权限进行操作");
                Object obj = method.invoke(target3, args);
                System.out.println("记录用户执行操作的用户信息、更改内容和时间等");
                return obj;
            }
        });

        Object obj3 = proxy3.find();
        System.out.println("find 返回对象：" + obj3.getClass());
        System.out.println("----------------------------------");
        proxy3.update();


    }
}

```

### Cglib代理

JDK动态代理要求target对象是一个接口的实现对象，假如target对象只是一个单独的对象，并没有实现任何接口，这时候就会用到Cglib代理(Code Generation Library)，即通过构建一个子类对象，从而实现对target对象的代理，因此目标对象不能是final类(报错)，且目标对象的方法不能是final或static（不执行代理功能）

#### Cglib代理案例

Cglib依赖的jar包
``` xml
<dependency>
          <groupId>cglib</groupId>
          <artifactId>cglib</artifactId>
          <version>3.2.10</version>
</dependency>
```
目标对象类AdminCglibService.java
``` Java

public class AdminCglibService {
    public void update() {
        System.out.println("修改管理系统数据");
    }

    public Object find() {
        System.out.println("查看管理系统数据");
        return new Object();
    }
}

```
代理类AdminServiceCglibProxy.java
``` Java
package com.lance.proxy.demo.service;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class AdminServiceCglibProxy implements MethodInterceptor {

    private Object target;

    public AdminServiceCglibProxy(Object target) {
        this.target = target;
    }

    //给目标对象创建一个代理对象
    public Object getProxyInstance() {
        //工具类
        Enhancer en = new Enhancer();
        //设置父类
        en.setSuperclass(target.getClass());
        //设置回调函数
        en.setCallback(this);
        //创建子类代理对象
        return en.create();
    }

    public Object intercept(Object object, Method method, Object[] arg2, MethodProxy proxy) throws Throwable {

        System.out.println("判断用户是否有权限进行操作");
        Object obj = method.invoke(target);
        System.out.println("记录用户执行操作的用户信息、更改内容和时间等");
        return obj;
    }
}
```
Cglib代理测试类CglibProxyTest.java

``` Java
package com.lance.proxy.demo.service;

public class CglibProxyTest {
    public static void main(String[] args) {

        AdminCglibService target = new AdminCglibService();
        AdminServiceCglibProxy proxyFactory = new AdminServiceCglibProxy(target);
        AdminCglibService proxy = (AdminCglibService)proxyFactory.getProxyInstance();

        System.out.println("代理对象：" + proxy.getClass());

        Object obj = proxy.find();
        System.out.println("find 返回对象：" + obj.getClass());
        System.out.println("----------------------------------");
        proxy.update();
    }
}

```