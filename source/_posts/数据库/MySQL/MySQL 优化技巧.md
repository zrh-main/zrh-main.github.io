---
title: MySQL 优化技巧
tags:
  - MySQL
categories:
  MySQL
---

##  代替in的方式
使用IN查询连续的数
 
SELECT *
FROM t
WHERE cid IN(955555, 955556, 955557, 955558, 955559);

41 rows in set (0.15 sec)
使用IN查询不连续的数

SELECT *
FROM (
    SELECT 1 AS cid UNION ALL
    SELECT 5000 UNION ALL
    SELECT 50000 UNION ALL
    SELECT 500000 UNION ALL
    SELECT 955559
) AS tmp, t
WHERE tmp.cid = t.cid;

41 rows in set (0.01 sec)
从上面可以看出上面使用UNION的方法生成一个临时表作为关联的主表。

拓展
要是MySQL有只带的一个行转列的函数那就完美了。这样我们就可以不用使用UNION了。
