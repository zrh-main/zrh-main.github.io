---
title: 内部类
date: 2021-12-07 11:47:00
  - Java重点
categories:
  Java
---

# 内部类

## 成员内部类
1. 概述:
    - 定义于另一个类的内部

2. 定义格式:
``` Java
public class 外部类名称{   
    class 成员内部类名称{

    }   
}
```      
3. 访问方式
    - 内部类访问外部类
      - 成员内部类可以无条件访问外部类的所有成员(属性,行为):包括private成员和静态成员
    - 外部类访问内部类
      - 创建内部类的对象,通过这个对象的引用访问
      - 创建内部类对象的方式,例:
      ``` Java
      外部类名.内部类名 对象名 = new 外部类型().new 内部类型();
      ```
4. 注意:
    1. 内部类是一个独立的类，在编译时内部类会被编译成独立的.class文件，但是前面冠以外部类的类名和$符号 
    2. 有同名的成员变量和方法时:
        - 默认访问成员内部类的成员
        - 访问外部类同名成员方式:```外部类.this.成员变量(成员方法)```
  
## 局部内部类

1. 概述:
    - 定义于另一个类的方法或作用域中的类
    - 不能有public,protected,private及static修饰符

2. 定义格式:
``` Java
public class 外部类名称{   
    修饰符  返回值类型  外部类方法（参数）{
        class 局部内部类名称{

        }   
    }
}
```

3. 访问方式
    - 局部内部类访问外部类
      - new 外部类名().成员(变量或方法)
    - 外部类访问内部类
      - 调用内部类所在的方法,方法内执行内部类的操作;例:
      ``` Java
      public class Demo1 {
          public void test(){
            class Poto{
                private String name;
                public void potoFun(){
                    System.out.println("这里是局部内部类方法");
                }
              }
             Poto poto =new Poto();
             poto.potoFun();
          }
          public static void main(String [] args){
            new Demo1().test();
          }
      }
      ```

## 权限修饰符在内部类中的使用
权限修饰符规则：
  1. 外部类： public /(default)
  2. 成员内部类  public/protected/(default)/private
  3. 局部内部类  什么都不能写

## 匿名内部类
1. 概述:
    - 是内部类的简化写法。
    - 本质是一个`带具体实现的` `父类或者父接口的` ` 匿名的` 子类对象

2. 格式:
``` Java
  new 父类名或者接口名(){ 
     
      @Override  // 方法重写 
      public void method() {
          // 执行语句 
      }
  };
```

注意:
  1. 匿名内部类必须继承一个父类或者实现一个父接口
  2. 匿名内部类在"创建对象"的时候,只能使用唯一一次.
  3. 如果希望多次创建对象,而且类的内容一样的话,那么就必须使用单独定义的实现类.

使用
  只使用一次对象时使用,
  在方法的形式参数是接口或者抽象类时，可以将匿名内部类作为参数传递;例
  ``` Java
  //  定义接口
  public interface class FlyAble{ 
    public abstract void fly();
}
  //  创建匿名内部类，并调用：
  public class InnerDemo {
      public static void main(String[] args) { 

          //创建匿名内部类,直接传递给showFly(FlyAble f) 
          showFly(new FlyAble(){ 
              public void fly() { //方法重写
                System.out.println("我飞了~~~"); 
            } 
          }); 
      }
      
      public static void showFly(FlyAble f) { 
          f.fly();                                    
      } 
  }
  ```

## 静态内部类
1. 概述
    - 有static关键字,不依赖外部类
    - 
2. 注意:
  不能使用外部类非静态成员

3. 格式:
``` Java
//创建静态内部类对象
外部类名.内部类名 xxx = new 外部类名.内部类名()
```