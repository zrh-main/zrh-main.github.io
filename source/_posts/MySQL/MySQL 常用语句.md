---
title: MySQL 常用语句
tags:
  - MySQL
categories:
  MySQL
---

## MySQL 服务
  - 启动服务`net start mysql`
  - 关闭服务`net stop mysql`
  
## 查看 MySQL 内部设置的编码
  - `show variables like 'character%';`

## DOS 命令窗口操作数据乱码问题的解决
  - 修改 client、connection、results 的编码为 GBK，保证和 DOS 命令行编码保持一致 
  - 同时设置三项:`set names gbk;`
  - 注意：退出 DOS 命令行就失效了，需要每次都配置

## MySQL 连接服务端
  - 本地连接
    - 方式一(密码可见)`mysql -u用户名 -p密码`
    - 方式二(密码不可见)
      - mysql -u root -p
      - 提示输入密码
      - 输入密码+回车
  - 远程连接
    - 方式一(密码可见)`mysql -h远程地址 -u用户名 -p密码`
    - 方式二(密码不可见)
      - mysql -h远程地址 -u用户名 -p
      - 提示输入密码
      - 输入密码+回车

## DOS命令窗口关闭连接
  - `quit` 或 `exit`
 
## 创建数据库
  - `create database 数据库名;`
  - 如果不存在则创建`create database if not exists 数据库名;`
  - 创建并指定字符集`create database 数据库名 character set 字符集;`

## 查询数据库
  - 查询全部数据库`show databases;`
  - 查询某个数据库的创建语句`show create database 数据库名;`
  - 查询正在使用的数据库`select database();`

## 修改数据库 
  - 修改数据库的字符集`alter database 数据库名 default character set 字符集;`

## 删除数据库
  - `drop database 数据库名;`

## 使用数据库或切换数据库
  - `use 数据库名;`

## 创建表
  - `create table 表名 (字段名1 字段类型1, 字段名2 字段类型2);`
  <font color='red'>注意: varchar类型数据需要设置长度,double类型需要设置长度和精度</font>
  例:`create table emp (name varchar(20),salary decimal(10,2),age tinyint(1));`

## 查看某个数据库的所有表
  - `show tables;`

## 查看表结构
  - `desc 表名;`

## 查看创建表的 SQL 语句
  - `show create table 表名;`

## 快速创建一个表结构相同的表
  - `create table 新表名 like 已存在表名;`

## 删除表
  - `drop table 表名;`

## 判断表是否存在，如果存在则删除表
  - `drop table if exists 表名;`

## 添加新的字段(列)
  - `alter table 表名 add 字段名 类型;`

## 修改字段(列)类型 MODIFY
  - `alter table 表名 modify 字段名 新的类型;`

## 修改字段(列)名 CHANGE
  - `alter table 表名 change 旧字段名 新字段名 类型;`

## 删除字段(列) DROP
  - `alter table 表名 drop 字段名;`

## 修改表名
  - `rename table 旧表名 to 新表名;`

## 修改表的字符集 character set
  - `alter table 表名 character set 字符集;`

## 插入全部字段
  - `insert into 表名 (字段名1, 字段名2, 字段名3) values (值1, 值2, 值3);`

## 插入多行数据
  - `insert into 表名 (字段名1, 字段名2, 字段名3) values (值1, 值2, 值3),(值1,值2,值3);`

## 蠕虫复制
  - 什么是蠕虫复制： 将一张已经存在的表中的数据复制到另一张表中。 
  - 语法格式: 将表名 2 中的所有的字段(列)复制到表名 1 中`insert into 表名1 select * from 表名2;`
  - 只复制部分列`insert into 表名1 (字段1, 字段2) select 字段1, 字段2 from 表名2;`

## 更新表记录(修改)
  - 不带条件修改数据`update 表名 set 字段名=值;` -- 修改所有的行
  - 带条件修改数据`update 表名 set 字段名=值 where 字段名=值;`

## 删除表记录 
  - 不带条件删除数据`delete from 表名;`或`truncate table 表名;`
  - 带条件删除数据`delete from 表名 where 字段名=值;`
  - truncate 和 delete 的区别：truncate 相当于删除表的结构，再创建一张表


## 查询指定字段
  - `select 字段1,字段2... from 表名;`推荐

## 查询所有
  - `select * from 表名;`影响性能

## 别名操作
  - 字段起别名
    - 优势： 显示的时候使用新的名字，并不修改表的结构
    - `select 字段1 as 别名1,字段2 as 别名2... from 表名;`
  - 表起别名
    - `select 字段名1,字段名2   from  表名 as 表别名;`
  - 字段和表起别名
    - `select 字段名1 as 别名1, 字段名2 as 别名2...  from  表名 as 表别名;`

## 清除重复结果
  - `select distinct 字段名 from 表名;`

## 查询结果参与运算
  - 字段数据和固定值运算
    - `select  字段1 + 固定值 from 表名;`
  - 字段数据1和字段数据2参与运算
    - `select 字段名1 + 字段名2 from 表名;`

## 条件查询
  - 对记录进行过滤
  - 格式:`select 字段 from 表名 where 条件;`
  - 条件使用比较运算符
    - =  检测两个值是否相等，相等返回true;例:`select name from student where age = 22`
    - != 检测两个值是否相等，不相等返回true;例:`select name from student where age != 22`
    - \> 检测左边的值是否大于右边的值, 若成立返回true;例:`select name from student where age > 22`
    - <  检测左边的值是否小于右边的值, 若成立返回true;例:`select name from student where age < 22`
    - \>= 检测左边的值是否大于或等于右边的值, 若成立返回true;例:`select name from student where age >= 22`
    - <= 检测左边的值是否小于或等于右边的值, 若成立返回true;例:`select name from student where age <= 22`
  - 条件使用逻辑运算符
    - and(与)	两个条件同时满足,例:`select name from student where age>20 and name='王五';`
    - or(或)	两个条件其中一个满足,例:`select name from student where age>20 or name='王五';`
    - not(非)	取反,例:`select name from student where age>20 and not name='王五';`
  - 范围 between .. and ...
    - <font color='red'>between后的值和and后的值都会包含</font>
      `select name from student where score between 84  and 89;`
  - 表示集合 in()
    - <font color='red'>in()内部的值都会包含</font>
      `select name from student where score in(83,89);`
  - 模糊查询 like
    - 通配符： `%`占位符，表示0个或多个;`_`占位符，表示一个
      - `select name from student where 字段 like '%王%';`
      - `select name from student where 字段 like '王_';`
  - 非空判断
    - `is null`  -- 查询空的
      - `select name from student where 字段 is  null;`
    - `is not null`  -- 查询非空的
      - `select name from student where 字段 is not null;`

## 排序 
  - 关键字:`order by`排序 `asc`升序和`desc`降序
  - 语法：
    - 升序:`select 字段 from 表名 where 条件 order by 字段 asc;`
    - 降序:`select 字段 from 表名 where 条件 order by 字段 desc;`
  - 组合排序：
    - 先根据第一个字段排序，如果第一个字段相同，再根据第二个字段排序，以此类推
      - `select 字段 from 表名 where 条件 order by 字段1 asc,字段2 desc`

## 聚合函数 
  - 数据库内置的一些函数，可以计算信息<font color='red'>(聚合函数会忽略null)</font>
  - 函数: max最大值 min最小值 avg平均值 count统计个数 sum求和
  - 语法：`select 聚合函数(字段名) from 表名;`

## 分组
  - 关键字: `group by`用来分组 `having`用来筛选分组后的数据(条件)
  - 语法: `select 字段 from 表名 where 条件  group by 分组字段 [having 条件];`
  - 注意: <font color='red'>若加上分组;查询的字段只能是聚合函数或者分组的字段</font>
  - where和having的区别?(面试题)
      - where是对分组前的条件进行限定。having是对分组后的内容进行限定
      - where后面不能加聚合函数，having后可以跟聚合函数

## 分页
  - 关键字: `limit 开始索引,个数`mysql的方言,只能在MySQL使用; 用来分页
  - 公式: 开始的索引 = (页码-1) * 每页的个数
  - 语法: `select name from student limit 2,6;`

## 数据库备份和还原
  1. 概述
    使用数据库存储数据的时，会对数据做一些操作。可以给数据做一些备份，防止数据丢失或者出现错误
  2. 备份格式：
    `mysqldump -u用户名 -密码 数据库>文件的路径;`
  3. 还原格式：
    `use 数据库;`
    `source 路径;`

## 数据库表的约束
  1. 概述
    对表中的数据进行限制，保证数据库的正确性，有效性和完成性。如果表添加了约束，不正确的数据将无法添加到表中。一般会在创建表的时候添加约束。(约束会加在字段后面，对某一列的数据进行限制)
  2. 分类
    唯一：数据唯一，不能重复。 `unique`
    非空：不能为空。   `not null`
    主键 ： 非空+唯一(一个表中只能有一个主键) `primary key`
    `auto_increment`  (自增，一般会和主键一起使用)
    默认值 `default` ，给某一列默认值。

## 外键
  - 检查约束（注：mysql不支持）