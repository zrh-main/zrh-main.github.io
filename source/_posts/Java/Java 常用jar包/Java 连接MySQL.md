---
title: Jdbc连接MySQL
date: 2022-2-11 14:56:43
tags:
  - Java jar包
categories: Java
---

## 概述
JDBC 规范定义接口，具体的实现由数据库厂商实现
JDBC 是 Java 访问数据库的标准规范，真正操作数据库还需要具体的实现类，也就是数据库驱动
每个数据库厂商根据自家数据库的通信格式编写好自己数据库的驱动。所以只需要调用 JDBC 接口中的方法即可，数据库驱动由数据库厂商提供。

## 使用步骤

1. 导入驱动jar包
  - 项目下新建lib(插件)文件夹
  - 将mysql驱动jar包解压至lib文件夹
2. 注册驱动
  - `Class.forName("com.mysql.jdbc.Driver");`实际调用下方语句;下方语句不需要写
  - DriverManager.registerDriver(new Driver()); //注册数据库驱动
3. 获取连接对象
  - 使用用户名、密码、URL 得到连接对象
    `Connection connection = DriverManager.getConnection(url,用户名,密码);`
4. 获取执行sql对象或获取预编译sql对象
  - 获取执行sql对象
    `Statement stat = conn.createStatement();`
  - 获取预编译sql对象
    `PreparedStatement prepareStatement = connection.prepareStatement("insert into student values (null,?,?,?)");`
5. 执行sql
  - 增,删,改语句使用executeUpdate 执行sql,结果成功为影响的行数,执行失败返回0
    `int i = stat.executeUpdate("delete from student where name = '陈七'");`
  - 查询语句使用executeQuery方法执行sql,结果是ResultSet类型的结果集对象
    `ResultSet rs = stat.executeQuery( "select * from user where username = '"+username+"' and password = '" + password +"'");`

6. 处理结果

``` Java
//  rs为查询的结果集对象
while(rs.next()){//next()判断下一个位置有没有结果,有则true,无则false;
  int id = rs.getInt("id");//从结果集中获取值
  String name = rs.getString("name");
  double score = rs.getDouble("score");
  String sex = rs.getString("sex");
  Student stu = new Student(id,name,score,sex);
  list.add(stu);
}
```
7. 释放资源
``` Java
rs.close();
stat.close();
conn.close();
```
## 事务
`void setAutoCommit(boolean autoCommit)`//参数是 true 或 false;如果设置为 false，表示关闭自动提交，相当于开启事务
`void commit()`	提交事务
`void rollback()`	回滚事务

## 使用预编译sql对象(PreparedStatement)
 
``` Java
PreparedStatement preparedStatement = connection.prepareStatement("select id from user where username=? and password=?");
//使用PreparedStatement类型的对象需要填充占位符
//占位符的填充位置从1开始,类型为要填充的数据类型
preparedStatement.setString(1,username);
preparedStatement.setString(2,password);
 
ResultSet resultSet = preparedStatement.executeQuery();
if(resultSet.next()){
   return  resultSet.getInt("id");
}
```
