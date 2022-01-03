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