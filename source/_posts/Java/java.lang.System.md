---
title: System
tags:
  - Java常用类
categories:
  - Java
---

# System
概述
  表示和系统操作相关的类。里面的方法都是静态的。

方法
  - static long currentTimeMillis()
      获取当前系统时间的毫秒值
  - static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
      实现数组拷贝
        参数1： 原数组
        参数2： 原数组中第一个要拷贝的数组元素的索引
        参数3： 目标数组
        参数4： 目标数组中第一位要放置的位置的索引
        参数5： 长度

        | 参数序号  | 参数名称  | 参数类型 | 参数含义             |
        | -------- | -------- | -------- | ------------------- |
        | 1        | src      | Object   | 源数组               |
        | 2        | srcPos   | int      | 源数组索引起始位置    |
        | 3        | dest     | Object   | 目标数组             |
        | 4        | destPos  | int      | 目标数组索引起始位置  |
        | 5        | length   | int      | 复制元素个数         |

  - staitc void exit(int n)
      结束jvm的执行,一般用于结束系统中。       


方法案例

``` Java
public class Demo {
    public static void main(String[] args) {

        //  获取当前系统时间
        long currentSystemTimeMs = System.currentTimeMillis();
        System.out.println(currentSystemTimeMs);

        //  将数组中指定的数据拷贝到另一个数组中
        int[] arr = {21, 13, 6, 8, 55};
        int[] arr2 = new int[8];
        System.arraycopy(arr, 0, arr2, 1, 4);
        for (int i = 0; i < arr2.length; i++) {
            System.out.println(arr2[i]);
        }

        //  结束jvm的执行
        int i = 1;
        for (; ; ) {
            if (i == 2000) {
                System.exit(0);
            }
            System.out.println(i);
            i++;
        }
    }
}

```
