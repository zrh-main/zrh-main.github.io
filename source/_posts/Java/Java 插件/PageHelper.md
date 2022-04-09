---
title: PageHelper
tags:
  - Java 插件
categories:
  - Java
---

## 概述
PageHelper是一款开源的mybatis分页插件，它支持基本主流与常用的数据库，例如mysql、
oracle、mariaDB、DB2、SQLite、Hsqldb等。
本项目在 github 的项目地址：https://github.com/pagehelper/Mybatis-PageHelper
本项目在 gitosc 的项目地址：http://git.oschina.net/free/Mybatis_PageHelper

## 使用

### 集成
引入分页插件有2种方式，推荐使用 Maven 方式。
1. 引入 Jar 包
https://oss.sonatype.org/content/repositories/releases/com/github/pagehelper/pagehelper/
http://repo1.maven.org/maven2/com/github/pagehelper/pagehelper/
由于使用了sql 解析工具，你还需要下载 jsqlparser.jar：
http://repo1.maven.org/maven2/com/github/jsqlparser/jsqlparser/0.9.5/
2. 使用 Maven
在 pom.xml 中添加如下依赖：
``` xml
<dependency>
<groupId>com.github.pagehelper</groupId>
<artifactId>pagehelper</artifactId>
<version>5.1.2</version>
</dependency>
```
配置
新版拦截器是 `com.github.pagehelper.PageInterceptor`
`com.github.pagehelper.PageHelper` 是一个特殊的 dialect 实现类，是分页插件的默认实现类，提供了和以前相同的用法

在 MyBatis 配置 xml 中配置拦截器插件
``` xml
<!-- 
plugins在配置文件中的位置必须符合要求，否则会报错，顺序如下:
properties?, settings?, 
typeAliases?, typeHandlers?, 
objectFactory?,objectWrapperFactory?, 
plugins?, 
environments?, databaseIdProvider?, mappers?
-->
<plugins>
<!-- com.github.pagehelper为PageHelper类所在包名 -->
<plugin interceptor="com.github.pagehelper.PageInterceptor">
<!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->
<property name="param1" value="value1"/>
 </plugin>
</plugins>
```
在 Spring 配置文件中配置拦截器插件;使用 spring 的属性配置方式，可以使用 plugins 属性像下面这样配置：
``` xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <!-- 注意其他配置 -->
  <property name="plugins">
    <array>
      <bean class="com.github.pagehelper.PageInterceptor">
        <property name="properties">
          <!--使用下面的方式配置参数，一行配置一个 -->
          <value>
            params=value1
          </value>
        </property>
      </bean>
    </array>
  </property>
</bean>
```

3.2.3 分页插件参数介绍
1. helperDialect ：分页插件会自动检测当前的数据库链接，自动选择合适的分页方式。 你可以配置
helperDialect 属性来指定分页插件使用哪种方言。配置时，可以使用下面的缩写值：
oracle , mysql , mariadb , sqlite , hsqldb , postgresql , db2 , sqlserver , informix , h2 , sqlserver201
2 , derby
特别注意：使用 SqlServer2012 数据库时，需要手动指定为 sqlserver2012 ，否则会使用 SqlServer2005 的
方式进行分页。
你也可以实现 AbstractHelperDialect ，然后配置该属性为实现类的全限定名称即可使用自定义的实现方
法。
2. offsetAsPageNum ：默认值为 false ，该参数对使用 RowBounds 作为分页参数时有效。 当该参数设置为
true 时，会将 RowBounds 中的 offset 参数当成 pageNum 使用，可以用页码和页面大小两个参数进行分
页。
3. rowBoundsWithCount ：默认值为 false ，该参数对使用 RowBounds 作为分页参数时有效。 当该参数设置
为 true 时，使用 RowBounds 分页会进行 count 查询。
4. pageSizeZero ：默认值为 false ，当该参数设置为 true 时，如果 pageSize=0 或者 RowBounds.limit = 
0 就会查询出全部的结果（相当于没有执行分页查询，但是返回结果仍然是 Page 类型）。
5. reasonable ：分页合理化参数，默认值为 false 。当该参数设置为 true 时， pageNum<=0 时会查询第一
页， pageNum>pages （超过总数时），会查询最后一页。默认 false 时，直接根据参数进行查询。
6. params ：为了支持 startPage(Object params) 方法，增加了该参数来配置参数映射，用于从对象中根据属
性名取值， 可以配置 pageNum,pageSize,count,pageSizeZero,reasonable ，不配置映射的用默认值， 默认
值为
pageNum=pageNum;pageSize=pageSize;count=countSql;reasonable=reasonable;pageSizeZero=pageSizeZero
。
7. supportMethodsArguments ：支持通过 Mapper 接口参数来传递分页参数，默认值 false ，分页插件会从查
询方法的参数值中，自动根据上面 params 配置的字段中取值，查找到合适的值时就会自动分页。 使用方法
可以参考测试代码中的 com.github.pagehelper.test.basic 包下的 ArgumentsMapTest 和
ArgumentsObjTest 。
8. autoRuntimeDialect ：默认值为 false 。设置为 true 时，允许在运行时根据多数据源自动识别对应方言
的分页 （不支持自动选择 sqlserver2012 ，只能使用 sqlserver ），用法和注意事项参考下面的场景五。
9. closeConn ：默认值为 true 。当使用运行时动态数据源或没有设置 helperDialect 属性自动获取数据库类
型时，会自动获取一个数据库连接， 通过该属性来设置是否关闭获取的这个连接，默认 true 关闭，设置为
false 后，不会关闭获取的连接，这个参数的设置要根据自己选择的数据源来决定。
3.2.4.基本使用
PageHelper的基本使用有6种，大家可以查看文档，最常用的有两种
