---
title: JDBC连接池
date: 2021-12-28 16:58:39
tags:
  - Java重点
categories:
  Java
---

## 概述
  是一种容器;使用集合来存储连接对象
  属于DriverManager的强化版;用来替代DriverManager
  接口:`javax.sql.DataSource`定义基础获取数据库连接的接口
  接口:`javax.sql.ConnectionPoolDataSource`：定义从数据库连接池中获取连接的接口
  接口:`javax.sql.XADataSource`：定义获取分布式事务连接的接口。一般少有直接使用 XA 分布式事务，具体原因参考分布式 2PC、3PC 事务模型

## 优势
  1. 可以节省创建连接和释放连接的时间，提高效率
  2. 同一个连接可以被多次使用
  3. 可以进行连接的管理。使用连接池之后，close方法不再是关闭连接，而是归还到连接池

## 连接池实现
### 有各种第三方实现:
  - c3p0  
    - c3p0是一个开放源代码的JDBC连接池，它在lib目录中与Hibernate一起发布,包括了实现jdbc3和jdbc2扩展规范说明的Connection 和Statement 池的DataSources 对象
    - c3p0有自动回收空闲连接功能
  - dbcp  
    - 依赖Jakarta commons-pool对象池机制的数据库连接池.DBCP可以直接的在应用程序中使用，Tomcat的数据源使用的就是DBCP
    - dbcp没有自动回收空闲连接的功能
  - druid 
    - 阿里提供的连接池技术

### 第三方实现的使用
  - c3p0
    1. 导入c3p0的jar包以及c3p0的依赖包和jdbc的jar包
    2. 编写配置文件,配置文件类型可以是properties和xml
      - 名称：c3p0.properties 或c3p0-config.xml;**c3p0会自动扫描配置文件,所以名称必须是这两种**
    3.  创建数据连接池对象(数据源对象)
        `DataSource  ds = new ComboPooledDataSource();`
  - druid
    1. 导入druid的jar包和jdbc的jar包
    2. 编写配置文件,名称随意,配置文件类型为properties;
      **注意:<font color='red'>该文件不能有多余的空格,否则会读取入项目中</font>**
      - 内容
        ``` Properties  
        # jdbc配置
        driverClassName=com.mysql.jdbc.Driver
        url=jdbc:mysql://127.0.0.1:3306/day04?useUnicode=true&characterEncoding=utf-8
        username=root
        password=root
        # 初始化连接数量
        initialSize=5
        # 最大连接数
        maxActive=10
        # 最大等待时间
        maxWait=3000
        ```
    3. 创建JdbcUtil工具类;内容如下:
      - 内容:
        ``` Java
        package cn.jdbc.util;
        import com.alibaba.druid.pool.DruidDataSourceFactory;
        import javax.sql.DataSource;
        import java.io.IOException;
        import java.io.InputStream;
        import java.sql.Connection;
        import java.sql.ResultSet;
        import java.sql.SQLException;
        import java.sql.Statement;
        import java.util.Properties;

        public class JdbcUtil {
            private static DataSource dataSource = null;
            static {
                //  创建Properties类对象
                Properties properties = new Properties();
                //  使用当前类的类加载器的getResourceAsStream("配置文件地址") 用于获取输入流Stream内容(读取配置文件)
                InputStream is = JdbcUtil.class.getClassLoader().getResourceAsStream("jdbc.properties");
                try {
                    //  使用Properties类对象的load(输入流Stream)加载输入流内容
                    properties.load(is);
                } catch (IOException e) {
                    e.printStackTrace();
                }
                try {
                    //  DruidDataSourceFactory  数据源工厂
                    //  createDataSource(加载配置文件后的Properties类对象)  创建数据源
                    dataSource = DruidDataSourceFactory.createDataSource(properties);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            /*
             *  获取连接对象
             */
            public static Connection getConnection() {
                try {
                    return dataSource.getConnection();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                return null;
            }

            /**
            * 释放资源
            */
            public static void close(Statement stmt, Connection conn) {
                close(null, stmt, conn);
            }

            /**
            * 释放资源
            */
            public static void close(ResultSet rs, Statement stmt, Connection conn) {
                if (rs != null) {
                    try {
                        rs.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (stmt != null) {
                    try {
                        stmt.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (conn != null) {
                    try {
                        conn.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
            }

            /**
            * 获取连接池(数据源)
            */
            public static DataSource getDataSource() {
                return dataSource;
            }
          }
        ```

## DBUtils
### 概述
  由apache基金会提供,对jdbc的操作进行了优化

### 使用
1. 创建QueryRunner类,传入jdbc数据源(连接池)
  `QueryRunner queryRunner = new QueryRunner(JdbcUtil.getDataSource());`

2. 使用
  - 增,删,改 调用`QueryRunner`对象的`update(sql语句,占位符填充数据...);`
  - 查单个 调用`QueryRunner`对象的`query(sql语句,new BeanHandler<>(Bean对象(又称实体类对象).class),占位符填充数据...);`
    `new BeanHandler<>(Bean对象(又称实体类对象).class)`;作用:创建Bean处理器对象传入bean对象,内部进行反射解析;将查询的数据映射在该类型对象的属性上,
  - 查多个   调用`QueryRunner`对象的`query(sql语句,new BeanListHandler<>(Bean对象(又称实体类对象).class),占位符填充数据...);`
    `new BeanListHandler<>(Bean对象(又称实体类对象).class)`;作用:创建Bean处理器集合对象传入bean对象,内部进行反射解析;将查询的数据映射在该类型对象的属性上,并将对象存入List集合中
  - 查个数
    ``` Java
      //  ScalarHandler用来查询个数如count查询;返回类型为Object或Long;一般整数类型为int;所以这里进行强转
      Object obj = queryRunner.query("select count(1)  from emp where ename like ? ", new ScalarHandler(),"%"+name+"%");
       Integer.parseInt(String.valueOf(obj));
    ```

