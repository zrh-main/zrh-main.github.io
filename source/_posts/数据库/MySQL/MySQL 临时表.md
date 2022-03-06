---
title: MySQL 临时表
tags:
  - MySQL
categories:
  MySQL
---
# 临时表

## 概述
用来保存一些临时数据 ; 临时表在MySQL 3.23版本中添加

## 特性
只在当前连接可见
使用 SHOW TABLES命令显示数据表列表时，无法看到 临时表

## 创建
使用关键字`TEMPORARY`

``` SQL
CREATE TEMPORARY TABLE tmp_test_2012-03-12 (
`id` INT UNIQUE PRIMARY key,
`name` varchar(20),
`age` TINYINT
);
```

用查询直接创建临时表：
``` SQL
CREATE TEMPORARY TABLE 临时表名 AS (SELECT *  FROM 旧的表名 LIMIT 0,10000);
```

## 删除
自动销毁: 关闭连接时，Mysql会自动删除表并释放空间
手动销毁: `drop table 表名`


   

