---
title: SpringClound Hystrix熔断器
date: 2022-01-03 19:49:57
tags:
  - Java框架
categories:
  Java
---

## 概述
Hystrix熔断器
`consumer（消费者）使用`
主页：https://github.com/Netflix/Hystrix/

## 作用
Hystrix是Netflix开源的一个延迟和容错库
用于隔离访问远程服务、第三方库，防止出现级联失败


## 雪崩问题
微服务中，服务间调用关系错综复杂，一个请求有可能调用多个微服务接口才可以实现，会形成非常复杂的调用链路
一次业务请求，需要调用A、P、H、I四个服务，这四个服务又可能调用其他服务，如果此时某个服务出现异常
例如微服务I发生异常，请求阻塞，用户不会得到响应，则tomcat这个线程不会释放，于是越来越多的用户请求到来，越来越多的线程会阻塞；服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其他服务都不可用，形成`雪崩效应`。

Hystrix解决雪崩问题的手段有两个：

- 线程隔离

- 服务熔断


## 开启熔断
在启动类上添加@EnableCircuitBreaker注解
``` Java
@EnableCircuitBreaker
@SpringBootApplication
@EnableDiscoveryClient
public class ConsumerApplication {
.......
```
## @SpringCloudApplication
SpringCloud提供了一个组合注解@SpringCloudApplication用来替换上方三个注解


## 降级逻辑

### @HystrixCommand
目标服务的调用出现故障，希望快速失败，给用户一个友好提示，因此需要提前编写好失败降级的处理逻辑，要使用@HystrixCommand来完成

```java
    @GetMapping("{id}")
    @HystrixCommand(fallbackMethod = "queryByIdFallback")
    public String queryById(@PathVariable("id")Long id) {
        String url = "http://user-service/user/"+id;
        String user = restTemplate.getForObject(url, String.class);
        return user;
    }
    public String queryByIdFallback(Long id) {
        return "网络开小差了，请稍后再试";
    }
```

注意:`因为熔断降级逻辑方法必须跟正常逻辑方法保证：相同的参数列表和返回值声明，失败逻辑中返回User对象没有太大意义，一般返回友好提示，所以吧queryById方法改为返回String，反正也是JSON数据，这样失败逻辑中返回一个错误说明会比较方便。`

说明：
- @HystrixCommand(fallbackMethod = "queryByIdFallback")用来声明一个降级逻辑的方法

测试：
  当提供者正常提供服务时，访问与以前一致，但是当我们将提供者停机时，会发现页面返回了降级处理。

## 默认的的Fallback
Fallback配置加到类上，实现默认的fallback  
使用注解`@DefaultProperties(defaultFallback = "queryByIdFallback")`;注意,默认执行的方法里不能有参数

## 超时设置
### 方式一
@HystrixCommand(commandProperties = {
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "2000")
    })
`这个配置会作用于全局所有方法`
### 方式二
通过配置文件的方式来实现超时
`hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds`来设置Hystrix超时时间
**推荐**

## 熔断原理
熔断器也叫断路器，其应为单词为：Circuit Breaker
![1525658640314](1525658640314-1565099920556.png)

## Hystrix熔断状态
- Closed：关闭状态（断路器关闭），所有请求都正常访问
- Open：打开状态（断路器打开），所有请求都会被降级，Hystrix会对请求情况计数，当一定时间内失败请求百分比达到阈值，则触发熔断器，断路器会完全关闭，默认失败比例的阈值是50%，请求次数最少不低于20次
- Half Open:半开状态，Closed状态不是永久的，关闭后会进入休眠时间（默认是5S）。随后熔断器会自动进入半开状态，此时会释放部分请求通过，若这些请求都是健康的，则会完全打开断路器，否则继续保持关闭，再次进入休眠计时。


## 熔断策略

circuitBreaker.requestVolumeThreshold =10 
circuitBreaker.sleepWindowInMilliseconds =10000 
circuitBreaker.errorThresholdPercentage =60 
解读
requestVolumeThreshold  触发熔断器最少请求次数默认是20
sleepWindowInMilliseconds   休眠时长，默认是5000毫秒
errorThresholdPercentage  触发熔断器最少占比默认50%

## 熔断器总结
当服务繁忙时，如果服务出现异常，不是粗暴的直接报错，而是返回一个友好的提示，虽然拒绝了用户的访问，但是会返回一个结果。
这就好比去买鱼，平常超市买鱼会额外赠送杀鱼的服务。等到逢年过节，超时繁忙时，可能就不提供杀鱼服务了，这就是服务的降级。
系统特别繁忙时，一些次要服务暂时中断，优先保证主要服务的畅通，一切资源优先让给主要服务来使用，在双十一、618时，京东天猫都会采用这样的策略。
