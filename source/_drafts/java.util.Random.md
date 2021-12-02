---
title: Random
tags:
  - Java重点类
categories:
  Java
---

## Random
概述: 此类的实例用于生成伪随机数

使用: 
1. 导包:   ```import java.util.Random;``` 
2. 创建:   ```Random random = new Random();```
3. 常用方法: ```random.nextInt(n) //生成0（包括）和 n（不包括）之间的随机数```  

案例:
``` Java
//生成m-n之间的随机数
//公式  random.nextInt(n-m+1)+m
import java.util.Random;
public class Demo {
    public static void main(String[] args) {
       //      创建一个随机数生成器。
        Random random = new Random();
        /**
         * @param int n 必须是正数
         * 生成m-n之间的随机数
         * 公式  random.nextInt(n-m+1)+m
         */
        System.out.println(random.nextInt(50-30+1)+30);
    }
}
```