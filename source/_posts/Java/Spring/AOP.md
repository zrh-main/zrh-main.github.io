---
title: AOP(面向切面)
tags:
  - Spring
categories:
  - Java
---



## 作用： 
在程序运行期间，不修改源码对已有方法进行增强

## 优势： 
减少重复代码 
提高开发效率 
维护方便

## 实现方式 
使用动态代理技术 

## 相关术语
- Joinpoint：连接点
含义:指那些被拦截到的点。在 spring 中,这些点指的是方法（被增强的方法）
- Pointcut：切入点。
含义:切入点是指我们要对哪些 Joinpoint 进行拦截的定义（业务层没有被增强的方法）
- Advice：通知。
含义:拦截到 Joinpoint 之后要做的事情 
通知的类型：前置通知,后置通知,异常通知,最终通知,环绕通知。 
- Introduction(引介): 
引介是一种特殊的通知在不修改类代码的前提下, Introduction 可以在运行期为类动态地添加一些方法或 Field。
- Target(目标对象): 
代理的目标对象。 
- Weaving(织入): 
是指把增强应用到目标对象来创建新的代理对象的过程。 
spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。 
- Proxy（代理对象）: 
一个类被 AOP 织入增强后，就产生一个结果代理对象
- Aspect(切面): 
是切入点和通知（引介）的结合


## 基于xml的aop配置

``` xml
<aop:config>
    <aop:aspect id="logAdvice" ref="logger">
      	<!--配置通知的类型，建立通知方法和切入点方法的关联-->
        <aop:before method="printLog" pointcut="execution(public void cn.dy.service.impl.AccountServiceImpl.addAccount())"></aop:before>
    </aop:aspect>
</aop:config>
```
切入点表达式：
  	关键字：execution(表达式)
    表达式：
       访问修饰符 返回值类型 包名.包名...类名.方法名(参数列表)
    例子：
       public void cn.dy.service.impl.AccountServiceImpl.addAccount()
全通配的写法：
* *..*.*(..)	
匹配任意返回值类型，类型包下的任意类的任意方法
访问权限修饰符可以省略  
void cn.dy.service.impl.AccountServiceImpl.addAccount()
返回值可以使用通配符，表示任意类型的返回值
* cn.dy.service.impl.AccountServiceImpl.addAccount()
包名可以使用通配符，表示任意包.（有几级，就得几个*）
* *.*.*.*.AccountServiceImpl.addAccount()
包名可以使用..表示当前包和子类
* *..AccountServiceImpl.addAccount()
类名和方法名也可以使用通配符
* *..*.*()
参数列表也可以直接写成参数类型
基本类型：   int
字符串：     java.lang.String
类型可以使用通配符*表示任意类型
类型使用..表示有无参数都可
实际开发中切入点表达式的通常写法：
切到业务层实现类下的所有方法
* cn.dy.service.impl.*.*(..)


## Advice 通知类型：
前置通知：`aop:before`
后置通知：`aop:after-returning`
异常通知：`aop:after-throwing`
最终通知：`aop:after`

环绕通知：`aop:around` 
**注意:通常情况下，环绕通知都是独立使用的**

## 基于注解的AOP配置
注解类型的通知类型会出现顺序错误的情况