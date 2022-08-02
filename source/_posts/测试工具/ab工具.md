---
title: ab工具
categories: 测试工具
date: 2021-12-22 14:53:58
---


## ab(压力测试工具)
httpd-tools
安装命令:（yum install -y httpd-tools）

## 使用
ab  -n（一次发送的请求数）  -c（请求的并发数） 访问路径

## 测试
在缓存(redis)中存num,初始值为0
使用缓存中的StringRedisTemplate获取到当前的num值
若num不为空,则对当前num+1;并写回缓存
若num为空,则直接返回
### 直接测试
  数据错误
### 测试本地锁(synchronized):
     单服务  
      结果：没有问题
     多服务(集群) 
      server.port 8206
      server.port 8226
      server.port 8236 
      结果：数据错误

## 结果
测试本地锁: 单击版本  没有问题

测试本地锁: 分布式式, 多个服务
        
有问题: synchronized 在一个tomcat中是没有问题的,在jvm内存  
   分布式:  多个tomcat中,  多个jvm虚拟中.没有办法控制线程安全问题.