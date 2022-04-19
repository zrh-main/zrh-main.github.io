---
title: SpringBoot+Mybatis
tags:
  - Java 框架
categories:
  - Java
---


## 4.5.整合mybatis

### 4.5.1.mybatis

SpringBoot官方并没有提供Mybatis的启动器，不过Mybatis[官方](https://github.com/mybatis/spring-boot-starter)自己实现了：

```xml
<!--mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
```

配置，基本没有需要配置的：

```properties
# mybatis 别名扫描
mybatis.type-aliases-package=com.doyens.pojo
# mapper.xml文件位置,如果没有映射文件，请注释掉
mybatis.mapper-locations=classpath:mappers/*.xml
```

需要注意，这里没有配置mapper接口扫描包，因此我们需要给每一个Mapper接口添加`@Mapper`注解，才能被识别。

 ![1574088323996](assets\1574088323996.png)

```java
@Mapper
public interface UserMapper {
}
```

user对象参照课前资料，需要通用mapper的注解：

![1540899330478](assets/1540899330478.png)

接下来，就去集成通用mapper。



### 4.5.2.通用mapper

通用Mapper的作者也为自己的插件编写了启动器，我们直接引入即可：

```xml
<!-- 通用mapper -->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.0.2</version>
</dependency>
```

不需要做任何配置就可以使用了。

```java
@Mapper
public interface UserMapper extends tk.mybatis.mapper.common.Mapper<User>{
}
```
