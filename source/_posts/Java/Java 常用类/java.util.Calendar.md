---
title: Calendar
tags:
  - Java常用类
  - Java 日期,时间,日历类
categories:
  - Java
---

# Calendar

## 概述
  - 表示日历的一个抽象类。
  - Calendar 对象能够生成为特定语言和日历风格实现日期-时间格式化所需的所有日历字段值

## 创建对象
  - static Calendar getInstance() 
  - 因为语言敏感性，Calendar类在创建对象时并非直接创建，而是通过此静态方法创建，返回子类对象;

## 常用字段
Calendar类中提供很多成员常量，代表给定的日历字段：  

| 字段值       | 含义                                  |
| ------------ | ------------------------------------- |
| YEAR         | 年                                    |
| MONTH        | 月（从0开始，可以+1使用）             |
| DAY_OF_MONTH | 月中的天（几号）                      |
| HOUR         | 时（12小时制）                        |
| HOUR_OF_DAY  | 时（24小时制）                        |
| MINUTE       | 分                                    |
| SECOND       | 秒                                    |
| DAY_OF_WEEK  | 周中的天（周几，周日为1，可以-1使用） |

## 常用方法
  - set(要设置的字段，值)
    - 设置时间
    - 注意：月份是从0开始的。
  - get(要获取的字段)
    - 获取时间
  - add(要设置的字段，数字)
    - 修改时间
    - 第二个参数中，正数表示加时间，负数表示减少时间。
  - Date getTime()
    - 把日历转成一个Date对象

## 常用方法案例

``` Java
import java.util.Calendar;

public class Demo1 {
    public static void main(String[] args) {
        //  创建Calendar日历对象
        Calendar calendar = Calendar.getInstance();

        //  设置年
        calendar.set(Calendar.YEAR, 2021);
        //  设置月 //该类中月份是从0开始的，范围是0-11;0为1月,11为12月
        calendar.set(Calendar.MONTH, 11);
        //  设置日
        calendar.set(Calendar.DAY_OF_MONTH, 8);

        //  获取
        System.out.println(calendar.get(Calendar.YEAR) + "年");
        System.out.println(calendar.get(Calendar.MONTH) + 1 + "月");
        System.out.println(calendar.get(Calendar.DAY_OF_MONTH) + "日");

        //  修改  add方法可以对指定日历字段的值进行加减操作，如果第二个参数为正数则加上偏移量，如果为负数则减去偏移量
        calendar.add(Calendar.DAY_OF_MONTH, -1);
        System.out.println(calendar.get(Calendar.YEAR) + "年");
        System.out.println(calendar.get(Calendar.MONTH) + 1 + "月");
        System.out.println(calendar.get(Calendar.DAY_OF_MONTH) + "日");

        //  转换为Date日期类型
        Date date = calendar.getTime();
        System.out.println(date);
    }
}

```