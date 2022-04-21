---
title: SpringClound zuul网关
date: 2022-01-03 19:49:57
tags:
  - Java框架
categories:
  Java
---

## 回顾
![1525674644660](1525674644660.png)
Spring Cloud Netflix中的Eureka实现了服务注册中心以及服务注册与发现；
而服务间通过Ribbon或Feign实现服务的消费以及均衡负载；
通过`Spring Cloud Config实现了应用多环境的外部化配置以及版本管理??`。
为了使得服务集群更为健壮，使用Hystrix的融断机制来避免在微服务架构中个别服务出现异常时引起的故障蔓延。

在该架构中，服务集群包含：内部服务Service A和Service B，他们都会注册与订阅服务至Eureka Server，而Open Service是一个对外的服务，通过均衡负载公开至服务调用方。

**存在问题:**
- 首先，破坏了服务无状态特点。
  - 为了保证对外服务的安全性，我们需要实现对服务访问的权限控制，而开放服务的权限控制机制将会贯穿并污染整个开放服务的业务逻辑，这会带来的最直接问题是，破坏了服务集群中REST API无状态的特点。
  - 从具体开发和测试的角度来说，在工作中除了要考虑实际的业务逻辑之外，还需要额外考虑对接口访问的控制处理。
- 其次，无法直接复用既有接口。
  - 当我们需要对一个即有的集群内访问接口，实现外部服务访问时，我们不得不通过在原有接口上增加校验逻辑，或增加一个代理调用来实现权限控制，无法直接复用原有的接口

**解决方案:**
zuul网关
## 概述
服务网关是微服务架构中一个不可或缺的部分。
通过服务网关统一向外系统提供REST API的过程中，除了具备服务路由、均衡负载功能之外，它还具备了`权限控制`等功能。
Spring Cloud Netflix中的Zuul就担任了这样的一个角色，为微服务架构提供了前门保护的作用，同时将权限控制这些较重的非业务逻辑内容迁移到服务路由层面，使得服务集群主体能够具备更高的可复用性和可测试性。
官网：https://github.com/Netflix/zuul
![1525675168152](1525675168152.png)
## Zuul加入后的架构
![1525675648881](1525675648881.png)
**描述**
不管是来自于客户端（PC或移动端）的请求，还是服务内部调用。一切对服务的请求都会经过Zuul这个网关，然后再由网关来实现 鉴权、动态路由等等操作。Zuul就是我们服务的统一入口。
## 依赖
pom.xml
``` xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.0.6.RELEASE</version>
</parent>

<dependencies>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
  </dependency>
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
  </dependency>
</dependencies>
<!-- SpringCloud的依赖 -->
<dependencyManagement>
  <dependencies>
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-dependencies</artifactId>
          <version>Finchley.SR2</version>
          <type>pom</type>
          <scope>import</scope>
      </dependency>
  </dependencies>
</dependencyManagement>
```
## @EnableZuulProxy 开启Zuul的功能
在启动类通过`@EnableZuulProxy`注解开启Zuul的功能：

```java
@SpringBootApplication
@EnableZuulProxy // 开启Zuul的网关功能
public class ZuulApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZuulApplication.class, args);
	}
}
```

## 配置
``` yml
server:
  port: 10010 #服务端口
spring: 
  application:    
    name: api-gateway #指定服务名
```

## 配置路由规则
方式一:
``` yml
zuul:
  routes:
    provider:                     # 这里是路由id，推荐和服务的名称一致
      path: /provider/**          # 这里是映射路径
      url: http://127.0.0.1:8082  # 映射路径对应的实际url地址
```
方式二:
``` yml
zuul:
  routes:
    provider:                 # 这里是路由id，推荐和服务的名称一致
      path: /provider/**      # 这里是映射路径
      serviceId: provider     # 指定服务名称  # 映射路径对应的实际url地址
```
方式三:
**`推荐使用此方式`**
``` yml
zuul:
  routes:
    provider: /provider/** # 这里是映射路径
```

项目中使用zuul配置路由规则之后；访问地址为：`localhost:10010/provider/`
### 路由前缀
``` yml
zuul:
  prefix: /api # 添加路由前缀
  routes:
    provider: /provider/** # 这里是映射路径
```

## zuul注册在eureka
在zuul的pom.xml中配置
``` xml
<!-- Eureka客户端 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```
启动类添加`@EnableDiscoveryClient`  
``` Java
@SpringBootApplication
@EnableZuulProxy // 启动Zuul组件
@EnableDiscoveryClient  //  开启Eureka客户端功能
public class ZuulApplication {
    public static void main(String[] args) {
        SpringApplication.run(ZuulApplication.class, args);
    }
}
```

## 过滤器
需要继承`ZuulFilter`抽象类并实现其中的方法
``` Java
package cn.dy.filter;

import com.netflix.zuul.ZuulFilter;
import com.netflix.zuul.context.RequestContext;
import com.netflix.zuul.exception.ZuulException;
import org.apache.http.HttpStatus;

import javax.servlet.http.HttpServletRequest;

/**
 * @author zrh
 * @date 2022/4/21
 * @apiNote
 */
public class TestFilter extends ZuulFilter {

    /**
     *过滤器类型===过滤时机
     *  值:
     *      pre     之前
     *      route   路由
     *      post    之后
     *      error   错误
     * @return
     */
    @Override
    public String filterType() {
        return "pre";
    }

    /**
     * 执行优先级
     * 值越小优先级越高
     * @return
     */
    @Override
    public int filterOrder() {
        return 5;
    }

    /**
     * 是否过滤
     * @return
     */
    @Override
    public boolean shouldFilter() {
        return true;
    }

    /**
     * 具体的过滤业务逻辑
     * @return
     * @throws ZuulException
     */
    @Override
    public Object run() throws ZuulException {
        //  获取到当前上下文
        RequestContext currentContext = RequestContext.getCurrentContext();
        //  获取到Request
        HttpServletRequest request = currentContext.getRequest();
        String token = request.getParameter("token");
        if(token==null){
            currentContext.setResponseStatusCode(HttpStatus.SC_FORBIDDEN);
            currentContext.setResponseBody("权限不足");
        }
        return null;    //  return null 代表不做任何操作
    }
}
```


## 3.10.负载均衡和熔断

Zuul中默认就已经集成了Ribbon负载均衡和Hystix熔断机制。但是所有的超时策略都是走的默认值，比如熔断超时时间只有1S，很容易就触发了。因此建议我们手动进行配置：   

```yaml
zuul:
  retryable: true
ribbon:
  ConnectionTimeOut: 500  # 连接超时时间(ms)
  ReadTimeout: 2000 # 通信超时时间(ms)
hystrix:
  command:
    default:
      execution:
         isolation:
            thread:
               timeoutInMilliseconds: 6000 # 熔断超时时长：6000ms
```

## 3.11.zuul的高可用

启动多个zuul服务，自动注册Eureka，形成集群如果是服务内部访问，你访问zuul，自动负载均衡，没问题，但是zuul更多的是外部访问，PC端，移动端等，他们无法通过Eureka进行负载均衡，那么该怎么办

此时我们会使用其他的服务网关，来对zuul进行代理，比如：nginx





Eureka，Ribbon，Hystrix，Feign，Zuul

Spring-Cloud config：统一配置中心，自动去Git拉取最新配置，缓存，使用git的Webhook钩子，去通知配置中心，说配置发生了变化，配置中心会通过消息总线去通知所有的微服务，更新配置。

Spring-Cloud-bus：消息总线

Spring-Cloud-stream：消息通信

Spring-Cloud-Hystrix-dashboard：容错统计，形成图形化界面

Spring-Cloud-sleuth：链路追踪，结合ZipKin

