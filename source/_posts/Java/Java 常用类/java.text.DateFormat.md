---
title: DateFormat
tags:
  - Java常用类
  - Java 日期,时间,日历类
categories:
  - Java
---


# DateFormat

## 概述:
  - 位于`java.text`;
  - 日期/时间格式化子类的抽象类，
  - 通过这个类可以帮我们完成日期和文本之间的转换(可以在Date对象与String对象之间进行来回转换)

## 使用
  由于DateFormat为抽象类，不能直接使用，所以使用子类`java.text.SimpleDateFormat`。

## SimpleDateFormat 使用
  这个类需要一个模式（格式）来指定格式化或解析的标准

### 构造方法：
SimpleDateFormat(String pattern)：
  - 用给定的模式和默认语言环境的日期格式符号构造SimpleDateFormat
  - 参数pattern是一个字符串，代表日期时间的自定义格式

### 格式规则
| 标识字母(区分大小写) | 含义 |
| ------------------ | ---- |
| y                  | 年   |
| M                  | 月   |
| d                  | 日   |
| H                  | 时   |
| m                  | 分   |
| s                  | 秒   |

### 代码
创建SimpleDateFormat对象
``` Java
public class Demo02SimpleDateFormat {
    public static void main(String[] args) {
        //日期/时间字符串格式
        String dateStringFormat = "yyyy-MM-dd HH:mm:ss"
        // 对应的日期格式如：2018-01-16 15:06:38
        DateFormat format = new SimpleDateFormat(dateStringFormat);

    }    
}
```
### 常用方法
String format(Date date)
  日期对象转字符串
Date parse(String s)
  字符串转日期对象

### 常用方法案例
format使用
``` Java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
/*
 Date对象转换成String
*/
public class DateFormatDemo {
    public static void main(String[] args) {
      //创建日期对象
        Date date = new Date();

        // 创建日期格式化对象,在获取格式化对象时可以指定风格
        DateFormat df = new SimpleDateFormat("yyyy年MM月dd日");

        //调用format将日期/时间对象转为字符串
        String str = df.format(date);
        System.out.println(str); // 2008年1月23日
    }
}
```

parse使用
``` Java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
/*
 把String转换成Date对象
*/
public class DateFormatDemo {
   public static void main(String[] args) throws ParseException {
        // 创建日期/时间格式化对象
        DateFormat df = new SimpleDateFormat("yyyy年MM月dd日");
        // 日期时间字符串
        String str = "2018年12月11日";
        //将String转为日期/时间对象
        Date date = df.parse(str);
        System.out.println(date); // Tue Dec 11 00:00:00 CST 2018
    }
}
```