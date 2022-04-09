---
title: REST(restful) 风格请求配置
tags:
  - SpringMVC
categories:
  - Java
---

## 概述
REST（英文：Representational State Transfer，简称 REST）
REST 并没有一个明确的标准，而像是一种设计的风格

## restful 的优点
结构清晰、符合标准、易于理解、扩展方便

## restful 的特性：
资源（Resources）：一个 URI（统一资源定位符）指向一个资源
表现层（Representation）：把资源具体呈现出来的形式，叫做它的表现层 （Representation）
状态转化（State Transfer）：每 发出一个请求，就代表了客户端和服务器的一次交互过程。
HTTP 协议，是一个无状态协议，即所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，
必须通过某种手段，让服务器端发生“状态转化”（State Transfer）。而这种转化是建立在表现层之上的，所以
就是 “表现层状态转化”。具体说，就是 

HTTP 协议里面，四个表示操作方式的动词：GET 、POST 、PUT、DELETE
分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源

restful 的示例：
/account/1 HTTP GET ： 得到 id = 1 的 account 
/account/1 HTTP DELETE： 删除 id = 1 的 account 
/account/1 HTTP PUT： 更新 id = 1 的 account

## @PathVaribale注解
作用：
  用于绑定 url 中的占位符。例如：请求 url 中 /delete/{id}，这个{id}就是 url 占位符。
  url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。
属性：
  value：用于指定 url 中占位符名称。
  required：是否必须提供占位符。
范围:
  方法参数
例:
Java代码
``` Java
控制器代码：
  /**
  * PathVariable 注解
  * @param user
  * @return
  */
  @RequestMapping("/usePathVariable/{id}")
  public String usePathVariable(@PathVariable("id") Integer id){
    System.out.println(id);
    return "success"; 
  }
```
jsp 代码：
``` jsp
<!-- PathVariable 注解 --> 
<a href="springmvc/usePathVariable/100">pathVariable 注解</a>
```

``` Java
控制器中示例代码： 
/**
* post 请求：保存
*/
@RequestMapping(value="/testRestPOST",method=RequestMethod.POST)
public String testRestfulURLPOST(User user){
  System.out.println("rest post"+user);
  return "success"; 
}
/**
* put 请求：更新
*/
@RequestMapping(value="/testRestPUT/{id}",method=RequestMethod.PUT)
public String testRestfulURLPUT(@PathVariable("id")Integer id,User user){
  System.out.println("rest put "+id+","+user);
  return "success"; 
}
/**
* post 请求：删除
*/
@RequestMapping(value="/testRestDELETE/{id}",method=RequestMethod.DELETE)
public String testRestfulURLDELETE(@PathVariable("id")Integer id){
  System.out.println("rest delete "+id);
  return "success";
}
/**
* post 请求：查询
*/
@RequestMapping(value="/testRestGET/{id}",method=RequestMethod.GET)
public String testRestfulURLGET(@PathVariable("id")Integer id){
  System.out.println("rest get "+id);
  return "success"; 
}
```