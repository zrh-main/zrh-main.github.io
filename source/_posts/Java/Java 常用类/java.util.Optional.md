---
title: Optional
date: 2021-12-19 18:51:18
tags:
  - Java常用类
categories:
  - Java
---

# Optional

## 概述
- Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象
- Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。
- Optional 类的引入很好的解决空指针异常。
- Java8引入
- 包:位于`java.util.Optional<T> `

## 常用静态方法

- 创建 Optional  实例
  - `Optional<T> of(T value)`
    - 返回具有Optional的当前非空值的Optional;明确对象不为null时使用
  - `Optional<T> ofNullable(T value)` 
    - 返回一个 Optional指定值的Optional;可能是 null 也可能是非 null时使用

- 访问 Optional 对象的值
  - `isPresent()`
    - 检查是否有值
  - `ifPresent(Consumer<? super T> consumer)`
    -  检查是否有值，还接受一个Consumer(消费者) 参数，如果对象不是空的，就执行传入的 Lambda 表达式

- 返回默认值
  Optional 类提供了 API 用以返回对象值，或者在对象为空的时候返回默认值
  - `T orElse(T other)`
    - 如果有值则返回该值，否则返回传递给它的参数值
  - `T orElseGet(Supplier<? extends T> other)`
    - 如果有值则返回该值，否则调用 other并返回该调用的结果
  - orElse() 和 orElseGet() 的不同之处
    - 对象为空而返回默认对象时，行为并无差异
    - 两个 Optional  对象都包含非空值，两个方法都会返回对应的非空值;orElse() 方法仍然创建了 T类型 对象。与之相反，orElseGet() 方法不创建 T类型 对象

## 常用方法案例
``` Java
//  实体类
public class User {
    private String name;
    private Integer age;
    private String email;

    public User() {
    }

    public User(String name, Integer age) {
        this.name = name;
        this.age = age;
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

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", email='" + email + '\'' +
                '}';
    }
}
//  运行类
public class Run {
    public static void main(String[] args) {
        User user = null;

        //  对象可能是 null 也可能是非 null时使用 ofNullable()
        Optional<User> opt2 = Optional.ofNullable(user);

        user = new User("张三", 18);

        //  明确对象不为 null  时使用 of()
        Optional<User> opt1 = Optional.of(user);

        User user1 = new User("张三", 18);
        //  检查是否有值是 ifPresent() 方法。
        //  该方法除了执行检查，还接受一个Consumer(消费者) 参数，如果对象不是空的，就执行传入的 Lambda 表达式：
        opt1.ifPresent( u ->  assertEquals(user1.getEmail(), u.getEmail()) );

        //  取回实际值对象使用 get() 方法
        System.out.println(opt1.get());
    }

    public static void assertEquals(String email1,String email2){
        System.out.println(email1);
        System.out.println(email2);
    }
}
```

## Optional  使用
- Optional 不是 Serializable。因此，它不应该用作类的字段。
- 在将其类型用作方法或构建方法的参数时;这样做会让代码变得复杂，完全没有必要
- Optional 主要用作返回类型;在获取到这个类型的实例后，如果它有值，你可以取得这个值，否则可以进行一些替代行为。
- Optional 类有一个非常有用的用例，就是将其与流或其它返回 Optional 的方法结合，以构建流畅的API。

## Optional  使用示例
使用 Stream 返回 Optional 对象的 findFirst() 方法：
``` Java
@Test
public void whenEmptyStream_thenReturnDefaultOptional() {
    List<User> users = new ArrayList<>();
    User user = users.stream().findFirst().orElse(new User("default", "1234"));
    assertEquals(user.getEmail(), "default");
}
```
总的来说，这个简单而强大的类有助于创建简单、可读性更强、比对应程序错误更少的程序

## 判空方法封装
``` java
    public String attributeIsNull(Element item, String str) {
        return Optional.ofNullable(item.selectSingleNode(str))
                .map(n -> n.getText())
                .orElse(StringUtils.EMPTY);
    }

```