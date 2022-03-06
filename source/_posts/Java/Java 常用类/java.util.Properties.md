---
title: Properties
tags:
  - Java常用类
categories:
  - Java
---
# Properties

## 概述
  - Java 语言的配置文件所使用的类
  - Xxx.properties 为Java 语言常见的配置文件，如数据库的配置 jdbc.properties, 系统参数配置 system.properties等
  - 位于`java.util.Properties`
  - 继承了Hashtable 类，以Map 的形式进行放置值， put(key,value) get(key)
  - 规则: 以key=value 的 键值对的形式进行存储值。 key值不能重复
  - 这个类是线程安全的：多个线程可以共享一个Properties对象，而不需要外部同步

## 注意
  - Xxx.properties文件内容案例
    内容是key=value的形式,<font color='red'>注意value后方不能存在空格;如果存在,读取在Java中的数据也会存在空格</font>
    ``` Poperties
    driver=com.mysql.jdbc.Driver
    url=jdbc:mysql://127.0.0.1:3306/db1?useUnicode=true&characterEncoding=utf-8
    username=root
    password=root
    ```

## 常用方法
  - String getProperty(String key) 
    使用此属性列表中指定的键搜索属性

## 案例
``` Java
import java.io.IOException;
import java.io.InputStream;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

//读取jdbc.properties配置文件
public class ReadJdbcConfig {

    private static Properties properties;
    //用Map集合存放properties文件下的配置信息
    private static Map<String, String> configMap;

    //配置文件只加载一次
    static {
        properties = new Properties();
        configMap = new HashMap<>();
        InputStream inputStream = null;
        try {
            inputStream = Thread.currentThread().getContextClassLoader().getResourceAsStream("jdbc.properties");
            properties.load(inputStream);
            Enumeration en = properties.propertyNames();
            while (en.hasMoreElements()) {
                String key = (String) en.nextElement();
                String value = properties.getProperty(key);
                configMap.put(key, value);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (inputStream != null) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    //获取配置文件里面的value值
    public static String getPropertyValue(String key) {
        return configMap.get(key);
    }
}

```



# 第五章 属性集

## 5.1 概述

`java.util.Properties ` 继承于` Hashtable` ，来表示一个持久的属性集。它使用键值结构存储数据，每个键及其对应值都是一个字符串。该类也被许多Java类使用，比如获取系统属性时，`System.getProperties` 方法就是返回一个`Properties`对象。

## 5.2 Properties类

### 构造方法

- `public Properties()` :创建一个空的属性列表。

### 基本的存储方法

- `public Object setProperty(String key, String value)` ： 保存一对属性。  
- `public String getProperty(String key) ` ：使用此属性列表中指定的键搜索属性值。
- `public Set<String> stringPropertyNames() ` ：所有键的名称的集合。

```java
public class ProDemo {
    public static void main(String[] args) throws FileNotFoundException {
        // 创建属性集对象
        Properties properties = new Properties();
        // 添加键值对元素
        properties.setProperty("filename", "a.txt");
        properties.setProperty("length", "209385038");
        properties.setProperty("location", "D:\\a.txt");
        // 打印属性集对象
        System.out.println(properties);
        // 通过键,获取属性值
        System.out.println(properties.getProperty("filename"));
        System.out.println(properties.getProperty("length"));
        System.out.println(properties.getProperty("location"));

        // 遍历属性集,获取所有键的集合
        Set<String> strings = properties.stringPropertyNames();
        // 打印键值对
        for (String key : strings ) {
          	System.out.println(key+" -- "+properties.getProperty(key));
        }
    }
}
输出结果：
{filename=a.txt, length=209385038, location=D:\a.txt}
a.txt
209385038
D:\a.txt
filename -- a.txt
length -- 209385038
location -- D:\a.txt
```

### 与流相关的方法

- `public void load(InputStream inStream)`： 从字节输入流中读取键值对。 

参数中使用了字节输入流，通过流对象，可以关联到某文件上，这样就能够加载文本中的数据了。文本数据格式:

```
filename=a.txt
length=209385038
location=D:\a.txt
```

加载代码演示：

```java
public class ProDemo2 {
    public static void main(String[] args) throws FileNotFoundException {
        // 创建属性集对象
        Properties pro = new Properties();
        // 加载文本中信息到属性集
        pro.load(new FileInputStream("read.txt"));
        // 遍历集合并打印
        Set<String> strings = pro.stringPropertyNames();
        for (String key : strings ) {
          	System.out.println(key+" -- "+pro.getProperty(key));
        }
     }
}
输出结果：
filename -- a.txt
length -- 209385038
location -- D:\a.txt
```

> 小贴士：文本中的数据，必须是键值对形式，可以使用空格、等号、冒号等符号分隔。

# 第六章改造JDBC连接属性配置



## 6.1.创建jdbc.properties文件

![1598354505898](img/1598354505898.png)

```properties
url=jdbc:mysql://localhost:3306/db3
user=root
password=root
driver=com.mysql.jdbc.Driver
```

## 6.2.读取配置文件

```java
import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.sql.*;
import java.util.Properties;

/**
 * JDBC工具类
 */
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;
    /**
     * 文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
     */
    static{
        //读取资源文件，获取值。
        try {
            //1. 创建Properties集合类。
            Properties pro = new Properties();
            //获取src路径下的文件的方式--->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            //URL是统一资源定位
            URL res  = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
           // System.out.println(path);ies
            //2. 加载文件
           // pro.load(new FileReader("D:\\IdeaProjects\\code\\day20_jdbc\\src\\jdbc.properties"));
            pro.load(new FileReader(path));

            //3. 获取数据，赋值
            url = pro.getProperty("url");
            user = pro.getProperty("user");
            password = pro.getProperty("password");
            driver = pro.getProperty("driver");
            //4. 注册驱动
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

```