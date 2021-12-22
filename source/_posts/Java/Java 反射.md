---
title: Java 反射
tags:
  - Java重点
categories: Java
date: 2021-12-22 14:09:58
---

# 反射

## 概述
  - 反射就是把Java类中的各个成分映射成一个个的Java对象
  - 即在运行状态中，对于任意一个类，都能够知道这个类的所以属性和方法；对于任意一个对象，都能调用它的任意一个方法和属性。这种动态获取信息及动态调用对象方法的功能叫Java的反射机制

## 反射机制的功能
- 在运行时判断任意一个对象所属的类。
- 在运行时构造任意一个类的对象。
- 在运行时判断任意一个类所具有的成员变量和方法。
- 在运行时调用任意一个对象的方法。
- 生成动态代理。

## 实现反射机制的类
 - Class类：代表一个类。 Field类：代表类的成员变量（成员变量也称为类的属性）。
 - Method类：代表类的方法。
 - Constructor类：代表类的构造方法。
 - Array类：提供了动态创建数组，以及访问数组的元素的静态方法。

## 反射案例

创建pojo类
``` Java
public class Student {
    private String name;
    private Integer age;
    private String grade;

    public Student() {
    }

    public Student(String name, Integer age, String grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getGrade() {
        return grade;
    }

    public void setGrade(String grade) {
        this.grade = grade;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", grade='" + grade + '\'' +
                '}';
    }
}
```
执行类
``` Java
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Run {
    public static void main(String[] args) throws Exception {
        Student student = new Student("张三", 18, "p2");
        Object copy = new Run().copy(student);
        System.out.println(copy);
    }

    //    自定义的copy方法用来创建一个和参数objcet同样类型的对象，然后把object对象中的所有属性拷贝到新建的对象中，并将其返回
    public Object copy(Object object) throws Exception {
        //  1. 获取object的类型
        Class classType = object.getClass();
        System.out.println("Class:" + classType.getName());

        //  2. 通过默认构造方法创建一个新的对象
        Object objectCopy = classType.getConstructor(new Class[]{}).newInstance(new Object[]{});

        //  3. 获得对象的所有属性
        Field fields[] = classType.getDeclaredFields();

        for (int i = 0; i < fields.length; i++) {
            Field field = fields[i];
            String fieldName = field.getName();
            String firstLetter = fieldName.substring(0, 1).toUpperCase();
            //获得和属性对应的getXXX()方法的名字
            String getMethodName = "get" + firstLetter + fieldName.substring(1);
            //获得和属性对应的setXXX()方法的名字
            String setMethodName = "set" + firstLetter + fieldName.substring(1);

            //获得和属性对应的getXXX()方法
            Method getMethod = classType.getMethod(getMethodName, new Class[]{});
            //获得和属性对应的setXXX()方法
            Method setMethod = classType.getMethod(setMethodName, new Class[]{field.getType()});

            //调用原对象的getXXX()方法
            Object value = getMethod.invoke(object, new Object[]{});
            System.out.println(fieldName + ":" + value);
            //调用拷贝对象的setXXX()方法
            setMethod.invoke(objectCopy, new Object[]{value});
        }
        return objectCopy;
    }
}
```