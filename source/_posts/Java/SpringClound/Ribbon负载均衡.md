---
title: SpringClound Ribbon负载均衡
date: 2022-01-03 19:49:57
tags:
  - Java框架
categories:
  Java
---

## 前言
实际环境中，会开启多个provider(提供者)的集群
此时获取的服务列表中就会有多个，这种情况下就需要编写负载均衡算法，在多个服务列表中进行选择。
Eureka中已经集成了负载均衡组件：`Ribbon`
`consumer（消费者）使用`
## 概述
![1525619257397](/assets/1525619257397.png)

## 开启负载均衡
Eureka中已经集成了Ribbon，所以无需引入新的依赖

在RestTemplate的配置方法上添加`@LoadBalanced`注解：
```java
@Bean
@LoadBalanced //  开启负载均衡
public RestTemplate restTemplate() {
    return new RestTemplate();
}
```

修改调用方式，不再手动获取ip和端口，而是直接通过服务名称调用：

```java
@Controller
@RequestMapping("consumer/user")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    //@Autowired
    //private DiscoveryClient discoveryClient; // 注入discoveryClient，通过该客户端获取服务列表

    @GetMapping
    @ResponseBody
    public User queryUserById(@RequestParam("id") Long id){
        // 获取服务提供方的服务列表，这里根据提供方名称;使用轮询的方式获取
        String baseUrl = "http://service-provider/user/" + id;
        User user = this.restTemplate.getForObject(baseUrl, User.class);
        return user;
    }

}
```

## 源码跟踪
`LoadBalancerInterceptor`根据service名称，获取到了服务实例的ip和端口


## 负载均衡策略
有轮询和随机两种策略;
由`IRule`接口定义负载均衡的规则接口
有以下实现：
  - `com.netflix.loadbalancer.RoundRobinRule`轮询实现
  - `com.netflix.loadbalancer.RandomRule`随机实现

默认为轮询的方式; 
修改负载均衡规则的配置入口需要在consumer的application.yml添加如下配置

``` yml
# 服务提供者名称
service-provider:
  ribbon:
    # 修改负载均衡规则为随机的方式
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

