---
title: MySQL 常用语句
tags:
  - MySQL
categories:
  MySQL
---

## 其它
### MySQL 服务
  - 启动服务`net start mysql`
  - 关闭服务`net stop mysql`
  
### 查看 MySQL 内部设置的编码
  - `show variables like 'character%';`

### DOS 命令窗口操作数据乱码问题的解决
  - 修改 client、connection、results 的编码为 GBK，保证和 DOS 命令行编码保持一致 
  - 同时设置三项:`set names gbk;`
  - 注意：退出 DOS 命令行就失效了，需要每次都配置

### MySQL 连接服务端
  - 本地连接
    - 方式一(密码可见)`mysql -u用户名 -p密码`
    - 方式二(密码不可见)
      - `mysql -u root -p`
      - 提示输入密码
      - 输入密码+回车
  - 远程连接
    - 方式一(密码可见)`mysql -h远程地址 -u用户名 -p密码`
    - 方式二(密码不可见)
      - `mysql -h远程地址 -u用户名 -p`
      - 提示输入密码
      - 输入密码+回车

### DOS命令窗口关闭连接
  - `quit` 或 `exit`

## 库操作

### 创建数据库
  - `create database 数据库名;`
  - 如果不存在则创建`create database if not exists 数据库名;`
  - 创建并指定字符集`create database 数据库名 character set 字符集;`
  - 不存在则新建数据库并指定字符集`CREATE DATABASE IF NOT EXISTS 数据库名 CHARACTER SET 'utf8mb4';`

### 查询数据库
  - 查询全部数据库`show databases;`
  - 查询某个数据库的创建语句`show create database 数据库名;`
  - 查询正在使用的数据库`select database();`

### 修改数据库 
  - 修改数据库的字符集`alter database 数据库名 character set 字符集;`

### 删除数据库
  - `drop database 数据库名;`

### 使用数据库(切换数据库)
  - `use 数据库名;`

* * *

## 表操作

### 创建表
  - `CREATE TABLE 表名(字段名 字段类型);`
  - 指定表引擎和字符集并添加表注释`CREATE TABLE 表名(字段名 字段类型) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT = '表注释';`
  - 不存在则新建`CREATE TABLE IF NOT EXISTS 表名(字段名 字段类型);`
  - 自增主键无符号并添加字段注释`CREATE TABLE IF NOT EXISTS 表名(id INT UNSIGNED AUTO_INCREMENT COMMENT '编号',PRIMARY KEY(`id`));`
  - 非空字段`CREATE TABLE 表名(字段名 字段类型  NOT NULL);`
  - 总结
      ``` SQL
          CREATE TABLE  IF NOT EXISTS `student` (
          `id` INT UNSIGNED AUTO_INCREMENT COMMENT '学生编号',
          `name` VARCHAR(15) NOT NULL COMMENT '学生姓名',
          `grade` VARCHAR(5) NOT NULL COMMENT '学生班级',
          `age` TINYINT(1) NOT NULL COMMENT '学生年龄',
          `score` DOUBLE(3,1) NOT NULL COMMENT '学生成绩',
          `start_date` DATE NOT NULL COMMENT '入学日期',
          PRIMARY KEY(`id`) 
          )ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT = '学生表';
          -- IF NOT EXISTS      不存在时新建
          -- UNSIGNED           无符号
          -- AUTO_INCREMENT     自增
          -- COMMENT            字段注释
          -- PRIMARY KEY(`id`)  指定id为主键
          -- ENGINE=InnoDB      引擎使用InnoDB
          -- DEFAULT CHARSET    默认编码为utf8mb4
          -- NOT NULL           非空
      ```
      <font c olor='red'>注意: varchar类型数据需要设置长度,double类型需要设置长度和精度</font>
  - 创建结构相同的表`create table 新表名 like 已存在表名;`
  - 创建带有复合主键的表`create table 表名(字段名 字段类型...,primary key(字段名,字段名2...));`

### 查看表
  - 查看某个数据库的所有表`show tables;`
  - 查看表结构`desc 表名;`
  - 查看创建表的 SQL 语句`show create table 表名;`

### 删除表
  - `drop table 表名;`
  - 判断表是否存在，如果存在则删除表`drop table if exists 表名;`

### 修改表
  - 修改表名`rename table 旧表名 to 新表名;`
  - 修改表的字符集`alter table 表名 character set 字符集;`

* * *

## 主键操作

### 表创建好之后添加主键：
  - `alter table 表名 add primary key(字段名);`

### 删除主键：
  - `alter table 表名 drop primary key;`

* * *

## 字段操作

### 添加新的字段(列)
  - `alter table 表名 add 字段名 类型;`

### 修改字段(列)类型
  - `alter table 表名 modify 字段名 新的类型;`

### 修改字段(列)名
  - `alter table 表名 change 旧字段名 新字段名 类型;`

### 删除字段(列)
  - `alter table 表名 drop 字段名;`

* * *

## 添加数据
  - 插入部分字段`insert into 表名 (字段名1, 字段名2, 字段名3) values (值1, 值2, 值3);`
  - 插入多行数据`insert into 表名 (字段名1, 字段名2, 字段名3) values (值1, 值2, 值3),(值1,值2,值3);`
  - 蠕虫复制:将一张已经存在的表中的数据复制到另一张表中
    - 语法格式: 将表名 2 中的所有的字段(列)复制到表名 1 中`insert into 表名1 select * from 表名2;`
    - 只复制部分列`insert into 表名1 (字段1, 字段2) select 字段1, 字段2 from 表名2;`
  - 添加数据返回自增的主键;在添加语句后执行:`select last_insert_id()`
## 更新表数据(修改)
  - 不带条件修改数据`update 表名 set 字段名=值;` -- 修改所有的行
  - 带条件修改数据`update 表名 set 字段名=值 where 字段名=值;`

## 更新某个日期字段为当前时间( now()函数 )
  -  `update 表名 set 字段名 = now() where 条件`

## 删除表数据 
  - 不带条件删除数据`delete from 表名;`或`truncate table 表名;`
  - 带条件删除数据`delete from 表名 where 字段名=值;`
  - truncate 和 delete 的区别：truncate 相当于删除表的结构，再创建一张表

## 查询表数据

### 基础查询
  - 查询指定字段`select 字段1,字段2... from 表名;`推荐
  - 查询所有`select * from 表名;`影响性能

### 别名操作
关键字: `as`可省略
优势： 显示的时候使用新的名字，并不修改表的结构,多个表时对性能有提高
  - 字段起别名`select 字段1 as 字段别名1,字段2 as 字段别名2... from 表名;`
  - 表起别名`select 字段名1,字段名2 from 表名 as 表别名;`
  - 字段和表起别名`select 字段名1 as 字段别名1, 字段字段名2 as 别名2...  from  表名 as 表别名;`

### 清除重复结果
  - 关键字:`distinct`
  - `select distinct 字段名 from 表名;`

### 查询结果参与运算
  - 字段数据和固定值运算
    - `select  字段1 + 固定值 from 表名;`
  - 字段数据1和字段数据2参与运算
    - `select 字段名1 + 字段名2 from 表名;`

### NULL值替换
  - coalesce(字段名,替换值,替换值)
  - IFNULL(字段名,替换值)
  - 如果字段名对应的值不是NULL,返回值，否则返回替换值;类型为数字或字符串
  - 例:`SELECT IFNULL(`mgr`,5) FROM `emp` WHERE `id`=7839;`若mgr的值不为null返回该值,否则返回5
  - 例:`SELECT coalesce(`mgr`, '总数') FROM `emp` WHERE `id`=7839;`若mgr的值不为null返回该值,否则返回总数

### 条件查询
  - 对记录进行过滤
  - 格式:`select 字段 from 表名 where 条件;`

#### 条件使用比较运算符
  - =  检测两个值是否相等，相等返回true;例:`select name from student where age = 22`
  - != 检测两个值是否相等，不相等返回true;例:`select name from student where age != 22`
  - \> 检测左边的值是否大于右边的值, 若成立返回true;例:`select name from student where age > 22`
  - <  检测左边的值是否小于右边的值, 若成立返回true;例:`select name from student where age < 22`
  - \>= 检测左边的值是否大于或等于右边的值, 若成立返回true;例:`select name from student where age >= 22`
  - <= 检测左边的值是否小于或等于右边的值, 若成立返回true;例:`select name from student where age <= 22`
#### 条件使用逻辑运算符
  - and(与)	两个条件同时满足,例:`select name from student where age>20 and name='王五';`
  - or(或)	两个条件其中一个满足,例:`select name from student where age>20 or name='王五';`
  - not(非)	取反,例:`select name from student where age>20 and not name='王五';`
#### 范围 between .. and ...
  - 关键字: 返回between ..  and ...
  - ..和...之间的所有数据;<font color='red'>between后的值和and后的值都会包含</font>
    `select name from student where score between 84  and 89;`
#### 表示集合 
  - 关键字:in()
  - <font color='red'>in()内部的值都会包含</font>
    `select name from student where score in(83,89);`
#### 模糊查询 like
  - 通配符： `%`占位符，表示0个或多个;`_`占位符，表示一个
    - `select name from student where 字段 like '%王%';`
    - `select name from student where 字段 like '王_';`
#### 非空判断
  - `is null`  -- 查询空的
    - `select name from student where 字段 is  null;`
  - `is not null`  -- 查询非空的
    - `select name from student where 字段 is not null;`

### 排序 
  - 关键字:`order by`排序 `asc`升序和`desc`降序
  - 语法：
    - 升序:`select 字段 from 表名  order by 字段 asc;`
    - 降序:`select 字段 from 表名  order by 字段 desc;`
  - 组合排序：
    - 先根据第一个字段排序，如果第一个字段相同，再根据第二个字段排序，以此类推
      - `select 字段 from 表名 where 条件 order by 字段1 asc,字段2 desc`

### 聚合函数 
  - 数据库内置的一些函数，可以计算信息<font color='red'>(聚合函数会忽略null)</font>
  - 函数: max最大值 min最小值 avg平均值 count统计个数 sum求和
  - 语法：`select 聚合函数(字段名) from 表名;`
  - `count(*)`或`count(1)`返回总记录数

### 分组
  - 关键字: `group by`用来分组 `having`用来筛选分组后的数据(条件) with rollup 在分组统计数据的基础上再统计汇总
  - 语法: `select 字段 from 表名 where 条件  group by 分组字段 with rollup having 条件;`
  - 注意: <font color='red'>若加上分组;查询的字段只能是聚合函数或者分组的字段</font>
  - 例:查询学生表以性别分组的 性别和平均成绩(成绩保留1位小数)`select sex,TRUNCATE (avg(score),1) from student group by sex;`
  - 分组后再使用条件筛选数据`select sex,TRUNCATE (avg(score),1) from student group by sex HAVING TRUNCATE (avg(score),1) > 80;`
  - 分组统计数据的基础上再进行统计汇总`SELECT name, SUM(signin) as signin_count FROM  employee_tbl GROUP BY name WITH ROLLUP;`
  - where和having的区别?(面试题)
      - where是对分组前的条件进行限定。having是对分组后的内容进行限定
      - where后面不能加聚合函数，having后可以跟聚合函数

### 正则
  - 关键字:`REGEXP`
  - 正则操作符
    - &nbsp;&nbsp;&nbsp;&nbsp;^	匹配开始位置
    - &nbsp;&nbsp;&nbsp;&nbsp;$	匹配结束位置
    - &nbsp;&nbsp;&nbsp;&nbsp;.	匹配除 "\n" 之外的任何单个字符。要包括 '\n' ，请使用'[.\n]' 的模式
    - &nbsp;&nbsp;&nbsp;&nbsp;[...]	字符集合。匹配所包含的任意一个字符
    - &nbsp;&nbsp;&nbsp;&nbsp;[^...]	负值字符集合。匹配未包含的任意字符
    - &nbsp;&nbsp;&nbsp;&nbsp;p1|p2|p3	匹配 p1 或 p2 或 p3
    - &nbsp;&nbsp;&nbsp;&nbsp;\*	匹配前面的子表达式零次或多次
    - &nbsp;&nbsp;&nbsp;&nbsp;\+	匹配前面的子表达式一次或多次
    - &nbsp;&nbsp;&nbsp;&nbsp;{n}	n 是一个非负整数。匹配确定的 n 次
    - &nbsp;&nbsp;&nbsp;&nbsp;{n,m}	m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次
  - 例:`SELECT 字段名 FROM 表名 WHERE 要筛选的字段 REGEXP '正则字符串';`

### 分页
  - 关键字: `limit 开始索引,个数`**mysql的方言**,只能在MySQL使用; 用来分页
  - 公式: 开始的索引 = (页码-1) * 每页的个数
  - 语法: `select 字段名 from student limit 开始索引,个数;`


## 特殊查询
  1. 根据班级分组，求出平均分最高的班级名称;
    - 获取最大值思路:1分组,2排序:降序,3取出第一条数据
    SELECT `grade`,AVG(`score`)`score` FROM `student` GROUP BY `grade`  ORDER BY `score` DESC LIMIT 0,1;
## 多思路查询  
  1. 查询product表中所有的电脑办公记录
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `category_name`='电脑办公';
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `category_name` in('电脑办公');
  2. 查询价格为800商品
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` = 800;
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` in(800);
  3. 查询价格不是800的所有商品
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` != 800;
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` NOT IN(800);
  4. 查询商品价格在200到1000之间所有商品
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` BETWEEN  200 AND 1000;
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` >=200 AND  `price`<=1000;
  5. 查询商品价格是200或800或者2000的所有商品
    - SELECT `id`,`name`,`price`,`category_name` FROM `product` WHERE `price` IN(200,800,2000);

## 外键

## 创建表时设置外键
  - `create table 表名(字段名,字段类型...,primary key(主键名),constraint '新建约束名' foreign key ('外键名') references 参照主键表名('主键名'));`

## 创建好的表添加外键约束
  - `alter table 表名 add constraimt 约束名称 foreign key(外键字段名) references 参照主键表名(主键字段名);`

## 删除外键
  - `alter table 表名 drop foreign key 外键名;`