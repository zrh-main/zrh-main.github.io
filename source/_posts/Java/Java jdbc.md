---
title: JDBC
date: 2021-12-14 10:58:39
tags:
  - Java重点
categories:
  Java
---
# JDBC

## 概述
- JDBC 规范定义接口，具体的实现由数据库厂商实现。
- JDBC 是 Java 访问数据库的标准规范，真正操作数据库还需要具体的实现类，也就是数据库驱动。
- 每个数据库厂商根据自家数据库的通信格式编写好自己数据库的驱动。所以只需要调用 JDBC 接口中的方法即可，数据库驱动由数据库厂商提供。

## 使用步骤
1. 导入驱动jar包
    - 项目下新建lib(插件)文件夹
    - 将mysql驱动jar包解压至lib文件夹
2. 注册驱动
    - `Class.forName("com.mysql.jdbc.Driver");`
      - Class.forName():
      `使用Class.forName()会将调用的类初始化，即调用class中的static块，并返回该类的Class对象`
      - Class.forName("com.mysql.jdbc.Driver")调用的是此操作
      `DriverManager.registerDriver(new Driver());//  注册数据库驱动` 
3. 获取连接对象
    - 使用用户名、密码、URL 得到连接对象
    ```Connection connection = DriverManager.getConnection(url,用户名,密码);```
4. 获取执行sql对象
    `Statement stat = conn.createStatement();`
5. 执行sql
    - 增,删,改  调用executeUpdate() 执行成功返回值为影响的行数,执行失败返回0
   `int i = stat.executeUpdate("delete from student where name = '陈七'");`
    - 查询      调用executeQuery()  执行成功返回查询结果集对象  ResultSet类型
    `ResultSet rs = stat.executeQuery( "select * from user where username = '"+username+"' and password = '" + password +"'");`
6. 处理结果
    - ``` Java
      //next()    结果集中存在下一个结果,返回true;否则返回false
      while(rs.next()){ 
        //  获取数据
        int id = rs.getInt("id");
        String name = rs.getString("name");
        double score = rs.getDouble("score");
        String sex = rs.getString("sex");
      }
      ```
7. 释放资源
    - ``` Java
      //  关闭结果集对象
      rs.close();
      //  关闭执行sql对象
      stat.close();
      //  关闭Connection连接对象
      conn.close();
      ```

## 案例
``` Java
public class Run {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        //  注册
        Class.forName("com.mysql.jdbc.Driver");Class.forName("com.mysql.jdbc.Driver");
 
        //  获取连接对象
        Connection connection = DriverManager.getConnection("jdbc:mysql://192.168.124.6/db1?characterEncoding=utf-8", "zrh", "imachinese;910");
 
        //  获取sql语句执行对象
        Statement statement = connection.createStatement();
 
        //  获取查询结果集对象
        ResultSet resultSet = statement.executeQuery("select * from student where name = '张三'");
 
        //  迭代打印查询到的信息
        while (resultSet.next()) {
            System.out.println(resultSet.getString("name"));
        }
 
        //  关闭连接
        resultSet.close();
        statement.close();
        connection.close();
    }
}
 ```

## (PreparedStatement)更换执行sql对象

### Statement的劣势
  - 存在sql注入问题
  - 性能方面损耗严重
    - 每次执行sql语句，相关数据库都要执行sql语句的编译

### PreparedStatement与Statement的区别
  - PreparedStatement是预编译的,对于批量处理可以大大提高效率
  - 支持占位符`?`可有效防止SQL注入

### 预编译语句的优势
  - 一次编译、多次运行，省去了解析优化等过程；此外预编译语句能防止sql注入

### PreparedStatement的使用
  ``` Java
    //  使用连接对象创建sql预编译语句对象
    PreparedStatement prepareStatement = connection.prepareStatement("insert into student values (null,?,?,?)");
    //  填充占位符  占位符从1开始,类型为要填充的数据类型
    prepareStatement.setString(1, "赵六");
    prepareStatement.setInt(2, 18);
    prepareStatement.setDouble(3, 95);
    // 执行sql
    int i = prepareStatement.executeUpdate();

  ```

## 事务

### JDBC中的事务
由Connection连接对象调用
- `void setAutoCommit(boolean autoCommit)`参数true或false;如果设置为 false，表示关闭自动提交，相当于开启事务
- `void commit()`	提交事务
- `void rollback()`	回滚事务

### 案例
``` Java
import java.sql.*;
public class JdbcUtil {
    private static final String driver = "com.mysql.jdbc.Driver";
    private static final String url = "jdbc:mysql://127.0.0.1/db1?characterEncoding=utf-8";
    private static final String username = "root";
    private static final String password = "root";

    static {
        try {
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            System.out.println("MySQL驱动注册失败!!!");
        }
    }

    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(url, username, password);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
            System.out.println("MySQL连接建立失败!!!");
        }
        return null;
    }
    public static void close(Statement statement, Connection connection) {
        close(null,statement,connection);
    }
    public static void close(ResultSet resultSet, Statement statement, Connection connection) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if( statement != null || connection != null){
            try {
                statement.close();
                connection.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

    }

}

import version3.utils.JdbcUtil;
import java.sql.*;
public class Run {
    //  获取连接对象
    static Connection connection = JdbcUtil.getConnection();
    //  创建学生初始化对象
    static Student student = null;
    //  创建执行sql预编译对象
    static PreparedStatement prepareStatement = null;
    //  创建查询结果集对象
    static ResultSet resultSet = null;

    public static void main(String[] args) {
//        insert();
//        update();
//        select();
//        selectAll();
//        delete();
    }

    //增
    public static void insert() {
        try {
            prepareStatement = connection.prepareStatement("insert into student values (null,?,?,?)");
            prepareStatement.setString(1, "赵六");
            prepareStatement.setInt(2, 18);
            prepareStatement.setDouble(3, 95);
            int i = prepareStatement.executeUpdate();
            if (i == 0) {
                System.out.println("操作失败");
                System.exit(1);
            }
            System.out.println("操作成功");
            System.out.println("影响的代码行数为:" + i);
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }

    //删
    public static void delete() {
        try {
            prepareStatement = connection.prepareStatement("delete from student where age=?");
            prepareStatement.setInt(1, 18);
            int i = prepareStatement.executeUpdate();
            if (i == 0) {
                System.out.println("操作失败");
                System.exit(1);
            }
            System.out.println("操作成功");
            System.out.println("影响的代码行数为:" + i);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


    //改
    public static void update() {
        try {
            prepareStatement = connection.prepareStatement("update  student set name=?,score=?  where age=18");
            prepareStatement.setString(1, "赵六update");
            prepareStatement.setDouble(2, 95);
            int i = prepareStatement.executeUpdate();
            if (i == 0) {
                System.out.println("操作失败");
                System.exit(1);
            }
            System.out.println("操作成功");
            System.out.println("影响的代码行数为:" + i);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static void select() {
        try {
            prepareStatement = connection.prepareStatement("select * from student where age=18");
            resultSet = prepareStatement.executeQuery();
            //  resultSet.next()  存在下一条数据时返回true,否则返回false
            while (resultSet.next()) {
                student = new Student(resultSet.getInt("id"), resultSet.getString("name"), resultSet.getInt("age"), resultSet.getDouble("score"));
            }
            System.out.println(student);
            System.out.println("操作成功");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

}

```