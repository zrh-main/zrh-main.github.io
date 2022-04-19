---
title: SpringBoot拦截器注入方式
tags:
  - Java 框架
categories:
  - Java
---

## starter
Spring Boot 将日常企业应用研发中的各种场景都抽取出来，做成一个个的 starter（启动器）
starter 中整合了该场景下各种可能用到的依赖，用户只需要在 Maven 中引入 starter 依赖，SpringBoot 就能自动扫描到要加载的信息并启动相应的默认配置。

## 约定大于配置
starter 提供了大量的自动配置，让用户摆脱了处理各种依赖和配置的困扰。所有这些 starter 都遵循着约定成俗的默认配置，并允许用户调整这些配置，即遵循“约定大于配置”的原则。

## 个别第三方技术
有部分 starter 是第三方技术厂商提供的，例如 druid-spring-boot-starter 和 mybatis-spring-boot-starter 等等。当然也存在个别第三方技术，Spring Boot 官方没提供 starter，第三方技术厂商也没有提供 starter。
可，而不需要额外导入 Web 服务器和其他的 Web 依赖。

## 使用
在 pom.xml 中引入 spring-boot-starter-web，示例代码如下。
``` xml
<!--SpringBoot父项目依赖管理-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.5</version>
    <relativePath/>
</parent>
<dependencies>
    <!--导入 spring-boot-starter-web-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
在以上 pom.xml 的配置中，引入依赖 spring-boot-starter-web 时，并没有指明其版本（version），这些版本信息是由 spring-boot-starter-parent（版本仲裁中心） 统一控制的。

## spring-boot-starter-parent
spring-boot-starter-parent 是所有 Spring Boot 项目的父级依赖，它被称为 Spring Boot 的版本仲裁中心，可以对项目内的部分常用依赖进行统一管理。
``` xml
<!--SpringBoot父项目依赖管理-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.5</version>
    <relativePath/>
</parent>
```
Spring Boot 项目通过继承 spring-boot-starter-parent 来获得一些合理的默认配置，它主要提供了以下特性：
默认 JDK 版本（Java 8）
默认字符集（UTF-8）
依赖管理功能
资源过滤
默认插件配置
识别 application.properties 和 application.yml 类型的配置文件

查看 spring-boot-starter- parent 的底层代码，可以发现其有一个父级依赖 spring-boot-dependencies。
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.4.5</version>
</parent>

spring-boot-dependencies 的底层代码如下。
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.4.5</version>
    <packaging>pom</packaging>
    ....
    <properties>
        <activemq.version>5.16.1</activemq.version>
        <antlr2.version>2.7.7</antlr2.version>
        <appengine-sdk.version>1.9.88</appengine-sdk.version>
        <artemis.version>2.15.0</artemis.version>
        <aspectj.version>1.9.6</aspectj.version>
        <assertj.version>3.18.1</assertj.version>
        <atomikos.version>4.0.6</atomikos.version>
        ....
    </properties>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.apache.activemq</groupId>
                <artifactId>activemq-amqp</artifactId>
                <version>${activemq.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.activemq</groupId>
                <artifactId>activemq-blueprint</artifactId>
                <version>${activemq.version}</version>
            </dependency>
            ...
        </dependencies>
    </dependencyManagement>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.codehaus.mojo</groupId>
                    <artifactId>build-helper-maven-plugin</artifactId>
                    <version>${build-helper-maven-plugin.version}</version>
                </plugin>
                <plugin>
                    <groupId>org.flywaydb</groupId>
                    <artifactId>flyway-maven-plugin</artifactId>
                    <version>${flyway.version}</version>
                </plugin>
                ...
            </plugins>
        </pluginManagement>
    </build>
</project>

以上配置中，部分元素说明如下：

dependencyManagement ：负责管理依赖；
pluginManagement：负责管理插件；
properties：负责定义依赖或插件的版本号。

spring-boot-dependencies 通过 dependencyManagement 、pluginManagement 和 properties 等元素对一些常用技术框架的依赖或插件进行了统一版本管理，例如 Activemq、Spring、Tomcat 等


## 常用启动器
1.	Spring Boot application starters
spring-boot-starter-thymeleaf
使用Thymeleaf视图构建MVC Web应用程序

spring-boot-starter-ws
使用Spring Web服务。1.4不推荐使用，推荐使用spring-boot-starter-web-services

spring-boot-starter-data-couchbase
Starter for using Couchbase document-oriented database and Spring Data Couchbase

spring-boot-starter-artemis
使用Apache Artemis启动JMS消息传递

spring-boot-starter-web-services
使用Spring Web服务

spring-boot-starter-mail
支持使用Java Mail和Spring Framework发送电子邮件

spring-boot-starter-data-redis
使用Redis键值数据存储与Spring Data Redis和Jedis客户端

spring-boot-starter-web
启动器构建web，包括RESTful，使用Spring MVC的应用程序。使用Tomcat作为默认嵌入式容器

spring-boot-starter-data-gemfire
Starter for using GemFire distributed data store and Spring Data GemFire

spring-boot-starter-activemq
使用Apache ActiveMQ启动JMS消息传递

spring-boot-starter-data-elasticsearch
使用Elasticsearch搜索和分析引擎和Spring Data Elasticsearch

spring-boot-starter-integration
Starter for using Spring Integration

spring-boot-starter-test
Spring Boot应用程序用于测试包括JUnit，Hamcrest和Mockito

spring-boot-starter-hornetq
使用HornetQ启动JMS消息传递。1.4已弃用，推荐使用spring-boot-starter-artemis

spring-boot-starter-jdbc
使用HikariCP连接池

spring-boot-starter-mobile
使用Spring Mobile构建Web应用程序的入门

spring-boot-starter-validation
使用Java Bean校验与Hibernate校验器

spring-boot-starter-hateoas
使用Spring MVC和Spring HATEOAS构建基于超媒体的RESTful Web应用程序的入门

spring-boot-starter-jersey
使用JAX-RS和Jersey构建RESTful Web应用程序的入门。 spring-boot-starter-web的替代品

spring-boot-starter-data-neo4j
使用Neo4j图数据库和Spring Data Neo4j

spring-boot-starter-websocket
使用Spring Framework的WebSocket支持构建WebSocket应用程序

spring-boot-starter-aop
使用Spring AOP和AspectJ进行面向方面编程

spring-boot-starter-amqp
使用Spring AMQP和Rabbit MQ的入门

spring-boot-starter-data-cassandra
使用Cassandra分布式数据库和Spring Data Cassandra

spring-boot-starter-social-facebook
使用Spring Social Facebook

spring-boot-starter-jta-atomikos
使用Atomikos进行JTA事务

spring-boot-starter-security
使用Spring Security

spring-boot-starter-mustache
使用Mustache视图构建MVC Web应用程序

spring-boot-starter-data-jpa
使用Spring Data JPA与Hibernate

spring-boot-starter
核心启动器，包括自动配置支持，日志记录和YAML

spring-boot-starter-velocity
使用Velocity视图构建MVC Web应用程序。1.4已弃用

spring-boot-starter-groovy-templates
使用Groovy模板视图构建MVC Web应用程序

spring-boot-starter-freemarker
使用FreeMarker视图构建MVC Web应用程序

spring-boot-starter-batch
使用Spring Batch

spring-boot-starter-redis
使用Redis键值数据存储与Spring Data Redis和Jedis客户端的入门。1.4已弃用，建议使用spring-boot-starter-data-redis

spring-boot-starter-social-linkedin
Stater for using Spring Social LinkedIn

spring-boot-starter-cache
支持使用Spring Framework的缓存

spring-boot-starter-data-solr
使用带有Spring Data Solr的Apache Solr搜索平台

spring-boot-starter-data-mongodb
使用MongoDB和Spring Data MongoDB

spring-boot-starter-jooq
使用jOOQ访问SQL数据库。 spring-boot-starter-data-jpa或spring-boot-starter-jdbc的替代方法

spring-boot-starter-jta-narayana
Spring Boot启动Narayana JTA

spring-boot-starter-cloud-connectors
启动者使用Spring Cloud连接器，简化了连接到云平台中的服务，如Cloud Foundry和Heroku

spring-boot-starter-jta-bitronix
使用Bitronix进行JTA事务

spring-boot-starter-social-twitter
使用Spring Social Twitter

spring-boot-starter-data-rest
使用Spring Data REST通过REST暴露Spring数据存储库

2.	Spring Boot production starters
spring-boot-starter-actuator
使用Spring Boot的Actuator，提供生产就绪的功能，以帮助您监视和管理您的应用程序

spring-boot-starter-remote-shell
使用CRaSH远程shell通过SSH监视和管理您的应用程序

3.	Spring Boot technical starters
spring-boot-starter-undertow
使用Undertow作为嵌入式servlet容器。 spring-boot-starter-tomcat的替代方法

spring-boot-starter-jetty
使用Jetty作为嵌入式servlet容器的。 spring-boot-starter-tomcat的替代方法

spring-boot-starter-logging
使用Logback进行日志记录。 默认日志启动器

spring-boot-starter-tomcat
使用Tomcat作为嵌入式servlet容器。 spring-boot-starter-web使用的默认servlet容器

spring-boot-starter-log4j2
使用Log4j2进行日志记录。 spring-boot-starter-logging的替代方法
