---
title: RestTemplate
tags:
  - Spring
categories:
  - Java
---

## 概述
Spring提供了一个RestTemplate模板工具类，对基于Http的客户端进行了封装，并且实现了对象与json的序列化和反序列化。
RestTemplate并没有限定Http的客户端类型，而是进行了抽象，目前常用的3种都有支持：
  - HttpClient
  - OkHttp
  - JDK原生的URLConnection（默认的）