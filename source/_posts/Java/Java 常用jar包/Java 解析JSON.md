---
title: JSON解析
date: 2022-2-11 14:56:43
tags:
  - Java jar包
categories: Java
---

## 概述
Jackson 是当前用的比较广泛的，用来序列化和反序列化 json 的 Java 的开源框架
Spring MVC 默认采用Jackson解析Json,出于最小依赖的考虑，Json解析第一选择应该是Jackson

## 优点
Jackson 社区相对比较活跃，更新速度也比较快， Jackson 所依赖的 jar 包较少 ，简单易用
Jackson 解析大的 json 文件速度比较快；Jackson 运行时占用内存比较低，性能比较好；Jackson 有灵活的 API，可以很容易进行扩展和定制。

## 版本
Jackson 的 1.x 版本的包名是 org.codehaus.jackson ,
当升级到 2.x 版本时，包名变为 com.fasterxml.jackson。

## 核心模块
Jackson 的核心模块由三部分组成

jackson-core，核心包，提供基于"流模式"解析的相关 API，它包括 JsonPaser 和 JsonGenerator。 Jackson 内部实现正是通过高性能的流模式 API 的 JsonGenerator 和 JsonParser 来生成和解析 json
jackson-annotations，注解包，提供标准注解功能；
jackson-databind ，数据绑定包， 提供基于"对象绑定" 解析的相关 API （ ObjectMapper ） 和"树模型" 解析的相关 API （JsonNode）；基于"对象绑定" 解析的 API 和"树模型"解析的 API 依赖基于"流模式"解析的 API

## 使用
Jackson 最常用的 API 是基于"对象绑定" 的 ObjectMapper：

ObjectMapper可以从字符串，流或文件中解析JSON，并创建表示已解析的JSON的Java对象。`将JSON解析为Java对象也称为从JSON反序列化Java对象`
ObjectMapper也可以从Java对象创建JSON。`从Java对象生成JSON也称为将Java对象序列化为JSON`
Object映射器可以将JSON解析为自定义的类的对象，也可以解析置JSON树模型的对象
之所以称为ObjectMapper是因为它将JSON映射到Java对象（反序列化），或者将Java对象映射到JSON（序列化）

## 案例
``` Java
Car类：
public class Car {
    private String brand = null;
    private int doors = 0;
    public String getBrand() { return this.brand; }
    public void   setBrand(String brand){ this.brand = brand;}
    public int  getDoors() { return this.doors; }
    public void setDoors (int doors) { this.doors = doors; }
}
将Json转换为Car类对象：
public class Run{
  public static void main(String [] args){
    ObjectMapper objectMapper = new ObjectMapper();
    String carJson ="{ \"brand\" : \"Mercedes\", \"doors\" : 5 }";
    try {
        Car car = objectMapper.readValue(carJson, Car.class);
        System.out.println("car brand = " + car.getBrand());
        System.out.println("car doors = " + car.getDoors());
    } catch (IOException e) {
        e.printStackTrace();
    }
  }
}
JSON字符串-->Java对象
调用ObjectMapper的相关方法进行转换
			1. readValue(json字符串数据,Class)
```

## 常用方法
``` Java
writeValue(File file,Object obj):Java对象转为json写入文件中
writeValue(Writer writer,Object obj):Java对象转为json,响应字符输出流
writeValue(OutputStream outputStream,Object obj):Java对象转为json,响应字节输出流
String writeValueAsString(Object obj):Java对象转为json字符串
```

## 常用注解
1. @JsonIgnore：排除某个属性
2. @JsonFormat：属性值格式化
* @JsonFormat(pattern = "yyyy-MM-dd")
