---
title: Bean实例
tags:
  - Spring
categories:
  - Java
---

## 概述
Spring 容器管理的Java对象称为bean对象

## Bean的作用域
singleton: 
  - 单例模式，在整个Spring IoC容器中，singleton作用域的Bean将只生成一个实例。
prototype: 
  - 每次通过容器的getBean()方法获取prototype作用域的Bean时，都将产生一个新的Bean实例
request: 
  - 对于一次HTTP请求，request作用域的Bean将只生成一个实例，这意味着，在同一次HTTP请求内，程序每次请求该Bean，得到的总是同一个实例。只有在Web应用中使用Spring时，该作用域才真正有效。
session：
  - 该作用域将 bean 的定义限制为 HTTP 会话。 只在web-aware Spring ApplicationContext的上下文中有效。
global session: 
  - 每个全局的HTTP Session对应一个Bean实例。在典型的情况下，仅在使用portlet context的时候有效，同样只在Web应用中有效。

**使用**
Spring默认使用singleton作用域
prototype作用域的Bean的创建、销毁代价比较大;而singleton作用域的Bean实例一旦创建，就可以重复使用;因此，**应该尽量避免将Bean设置成prototype作用域**

## Bean的自动装配
Spring容器检查XML配置文件内容，根据某种规则，为调用者Bean注入被依赖的Bean

**方式一**
通过<beans/>元素的default-autowire属性指定，该属性对配置文件中所有的Bean起作用
**方式二**
通过对<bean/>元素的autowire属性指定，该属性只对该Bean起作用

**autowire和default-autowire可以接受如下值**：
no: 不使用自动装配。Bean依赖必须通过ref元素定义。这是默认配置，在较大的部署环境中不鼓励改变这个配置，显式配置合作者能够得到更清晰的依赖关系。
byName: 根据setter方法名进行自动装配。Spring容器查找容器中全部Bean，找出其id与setter方法名去掉set前缀，并小写首字母后同名的Bean来完成注入。如果没有找到匹配的Bean实例，则Spring不会进行任何注入。
byType: 根据setter方法的形参类型来自动装配。Spring容器查找容器中的全部Bean，如果正好有一个Bean类型与setter方法的形参类型匹配，就自动注入这个Bean；如果找到多个这样的Bean，就抛出一个异常；如果没有找到这样的Bean，则什么都不会发生，setter方法不会被调用。
constructor: 与byType类似，区别是用于自动匹配构造器的参数。如果容器不能恰好找到一个与构造器参数类型匹配的Bean，则会抛出一个异常。
autodetect: Spring容器根据Bean内部结构，自行决定使用constructor或byType策略。如果找到一个默认的构造函数，那么就会应用byType策略。
当一个Bean既使用自动装配依赖，又使用ref显式指定依赖时，则显式指定的依赖覆盖自动装配依赖；对于大型的应用，不鼓励使用自动装配。虽然使用自动装配可减少配置文件的工作量，但大大将死了依赖关系的清晰性和透明性。依赖关系的装配依赖于源文件的属性名和属性类型，导致Bean与Bean之间的耦合降低到代码层次，不利于高层次解耦。
``` xml
<!--通过设置可以将Bean排除在自动装配之外-->
<bean id="" autowire-candidate="false"/>
```
``` xml
<!--除此之外，还可以在beans元素中指定，支持模式字符串，如下所有以abc结尾的Bean都被排除在自动装配之外-->
<beans default-autowire-candidates="*abc"/>
```

## bean 的作用范围和生命周期

**单例对象：scope="singleton"** 
一个应用只有一个对象的实例。它的作用范围就是整个引用 
生命周期： 
  - 对象出生：当应用加载，创建容器时，对象就被创建了 
  - 对象活着：只要容器在，对象一直活着
  - 对象死亡：当应用卸载，销毁容器时，对象就被销毁了

**多例对象：scope="prototype"**
每次访问对象时，都会重新创建对象实例
生命周期： 
  - 对象出生：当使用对象时，创建新的对象实例
  - 对象活着：只要对象在使用中，就一直活着
  - 对象死亡：当对象长时间不用时，被 java 的垃圾回收器回收了。GC垃圾回收机制 
``` xml
<bean id="accountService" class="com.doyens.service.impl.AccountServiceImpl"></bean> 
<bean id="accountDao" class="com.doyens.dao.impl.AccountDaoImpl"></bean> 
```

## 实例化 Bean 的三种方式

- 使用默认无参构造函数 
**根据默认无参构造函数来创建类对象;如果 bean 中没有默认无参构造函数，将会创建失败**
``` xml
<bean id="accountService" class="com.doyens.service.impl.AccountServiceImpl"/>
```

- 使用静态工厂的方法创建对象 
使用 StaticFactory 类中的静态方法 createAccountService 创建对象，并存入 spring 容器 
``` Java
public class StaticFactory { 
  public static IAccountService createAccountService(){ 
    return new AccountServiceImpl(); 
  } 
} 
```
id 属性：指定 bean 的 id，用于从容器中获取 
class 属性：指定静态工厂的全限定类名 
factory-method 属性：指定生产对象的静态方法 
``` xml
<bean id="accountService" class="com.doyens.factory.StaticFactory" factory-method="createAccountService"></bean>
```

- 使用实例工厂的方法创建对象 
此工厂创建对象，必须先有工厂实例对象，再调用方法 
``` Java
public class InstanceFactory { 
  public IAccountService createAccountService(){ 
    return new AccountServiceImpl(); 
  } 
}
 ```
先将工厂的创建交给 spring 来管理;然后让使用工厂的 bean 来调用方法
factory-bean 属性：用于指定实例工厂 bean 的 id。 
factory-method 属性：用于指定实例工厂中创建对象的方法。
``` xml
<bean id="instancFactory" class="com.doyens.factory.InstanceFactory"></bean> 
<bean id="accountService" factory-bean="instancFactory" factory-method="createAccountService"></bean>
```
