---
title: HTTP协议详解
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  其它
---

# HTTP协议

## 传输协议
  - 规定客户端和服务器通信时，发送数据的格式

## 概述
  - 超文本传输协议

##  特点
  1. 基于TCP/IP的高级协议
  2. 默认端口号：80
  3. 基于请求/响应模型：一次请求对应一次响应
  4. 无状态的：每次请求之间相互独立，不能交互数据

## 历史版本
  - 1.0 ：每一次请求都会建立新的连接
  - 1.1 ：复用连接（提高效率）
  - 2.0 : 
    - 连接复用 允许通过 HTTP/2 连接发起多重的请求-响应
    - 新增首部压缩 采用HPACK算法
    - 新增服务端推送

## 请求消息数据格式

### 请求行
  - 请求方式 资源url地址  请求的协议/版本
    例:&nbsp;&nbsp;GET /day16/hello.html HTTP/1.1

### 请求头(常用)
客户端告诉服务器的一些信息(请求头是键值对格式的)
  - `Accept`(请求的文件类型)&nbsp;&nbsp;例: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
  - `Accept-Encoding`(请求的编码格式)&nbsp;&nbsp;例: gzip, deflate
  - `Accept-Language`(语言)&nbsp;&nbsp;例: zh-CN,zh;q=0.9
  - `Content-Length`(请求的内容长度)&nbsp;&nbsp;例: 33
  - `Connection`(连接状态)&nbsp;&nbsp;例: keep-alive
    - keep-alive  设置该连接为常连接
  - `Host`(请求域名)&nbsp;&nbsp;例: localhost:8080
  - `User-Agent`(客户端的相关信息)&nbsp;&nbsp;例: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:95.0) Gecko/20100101 Firefox/95.0
  - `Referer`(请求来源)：
    - 作用：
      1. 防盗链
      2. 统计工作

### 请求空行
是一个空行 用来分割请求头和请求体的

### 请求体
  post请求的请求参数

## 响应消息数据格式

### 响应行
  - 协议/版本 响应状态码 状态码描述
    例:&nbsp;&nbsp;HTTP/1.1 200 描述...

### 响应头(常用)
服务器告诉客户端的一些信息(响应头是键值对格式的)
`Content-Length`(响应的内容长度)&nbsp;&nbsp;例: 19
`Content-Type`(响应的数据格式及编码格式)&nbsp;&nbsp;例: text/html;charset=utf-8
`Content-disposition`(响应数据的打开方式)&nbsp;&nbsp;例: in-line
  - in-line：默认值 ; 在当前页面内打开
  - attachment;filename=xxx 以附件的形式打开响应体=文件下载

### 响应空行
是一个空行 用来分割响应头和响应体的

### 响应内容
  给客户端传输的数据

## 常见请求方式：

## get请求与post请求的区别
  - GET （没有请求体）
    1. 请求的参数在url路径后
    2. 请求的url长度有限制
    3. 不太安全
  - POST
    1. 请求的参数在请求体中
    2. 请求的url长度没有限制
    3. 相对安全

## 常见的http状态码:
  - 200 ：请求成功
  - 301 ：资源移动。所请求资源自动到新的URL，浏览器自动跳转到新的URL
  - 304 ：请求缓存服务端的资源与客户端上一次请求的一致，不需要重新传输，客户端使用本地缓存的即可
  - 400 ：错误请求
  - 404 ：(资源丢失)未找到资源
  - 500 ：服务器内部错误

## 请求转发与请求重定向
转发和重定向都是实现页面跳转
  - 转发：在服务端内部的资源跳转方式,转发之后 当前操作的后续代码会执行
    1. 地址栏不发生变化
    2. 转发只能转发服务器内部的资源
    3. 转发是一次请求
  - 重定向：客户端操作,后续代码会执行
    1. 地址栏发生变化
    2. 重定向可以访问任意资源
    3. 重定向是两次请求