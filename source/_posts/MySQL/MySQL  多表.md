---
title: MySQL 多表
tags:
  - MySQL
categories:
  MySQL
---

# 多表

## 多表之间的关系
一对一：
  - 一般不用, 用一张表表示即可!
一对多：
  - 在多的一方建立字段, 指向一的一方的主键
多对多：
  - 如果两张表是多对多的关系，一般情况下会设置一张中间表，让中间表的两个字段分别去关联两张表的主键列

## 多表查询

### 内连接
**获取两个表中字段匹配关系的记录**
用左边表的记录去匹配右边表的记录，如果符合条件的则显示。如：从表.外键=主表.主键
1. 隐式内连接：
  - 没有 JOIN 关键字，条件使用 WHERE 指定
    例:
    ``` SQL
    SELECT e.`id`,e.`ename`,e.`salary`,j.`jname`,j.`description` FROM  emp e,job j WHERE e.`job_id` = j.`id`
    ```
2. 显示内连接：
  - 使用 `INNER JOIN ... ON` 语句, 可以省略 INNER 使用 `JOIN` 
    例:
    ``` SQL
    SELECT e.`id`,e.`ename`,e.`salary`,j.`jname`,j.`description` FROM  emp e JOIN job j ON e.`job_id` = j.`id`
    ```

### 外连接
  - LEFT JOIN（左连接）：使用 `LEFT OUTER JOIN ... ON`，OUTER 可以省略
    **获取左表所有记录，即使右表没有对应匹配的记录**
    用左边表的记录去匹配右边表的记录，如果符合条件的则显示；否则，显示 NULL
    可以理解为：在内连接的基础上保证左表的数据全部显示
    例:
    ``` SQL
    SELECT e.`id`,e.`ename`,e.`salary`,j.`jname`,j.`description` FROM  emp e LEFT JOIN job j ON e.`job_id` = j.`id`
    ```
  - RIGHT JOIN（右连接）：右外连接：使用 RIGHT OUTER JOIN ... ON，OUTER 可以省略
    **与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录**
    用右边表的记录去匹配左边表的记录，如果符合条件的则显示；否则，显示 NULL
    可以理解为：在内连接的基础上保证右表的数据全部显示
    例:
    ``` SQL
    SELECT e.`id`,e.`ename`,e.`salary`,j.`jname`,j.`description` FROM job j RIGHT JOIN   emp e ON e.`job_id` = j.`id`
    ```
    
### UNION 操作符

MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。
``` SQL
SELECT `字段名1`,`字段名2`, ... `字段名n` FROM `表名1`
where `条件`
UNION [ALL | DISTINCT]
SELECT `字段名1`,`字段名2`, ... `字段名n` FROM `表名2`
where `条件`;
-- DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。
-- ALL: 可选，返回所有结果集，包含重复数据。
```

### 子查询

#### 概述
  1. 一个查询的结果做为另一个查询的条件
  2. 有查询的嵌套，内部的查询称为子查询
  3. 子查询要使用括号

#### 子查询结果的三种情况：
1. 子查询的结果是单行单列(一个值)
    - 子查询结果只要是单行单列，肯定在 WHERE 后面作为条件，父查询使用：比较运算符，如：> 、<、<>、= 等
2. 子查询的结果是多行单列(一列值)
    - 子查询结果是多列，则在 FROM 后面作为表进行二次查询;结果集类似于一个数组，父查询使用 IN 运算符
3. 子查询的结果是多行多列(多行多列值)
    - 子查询结果只要是多列，肯定在 FROM 后面作为表;子查询作为表需要取别名，否则这张表没有名称则无法访问表中的字段