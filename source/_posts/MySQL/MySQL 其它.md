---
title: MySQL 常用语句
tags:
  - MySQL
categories:
  MySQL
---




## 数据库备份和还原
  1. 概述
    使用数据库存储数据时，可以给数据做一些备份，防止数据丢失或者出现错误
  2. 备份格式：
    `mysqldump -u用户名 -密码 数据库>文件的路径;`
  3. 还原格式：
    `use 数据库;`
    `source 路径;`
