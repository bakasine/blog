---
title: SpringCloud 笔记二
date: 2020-03-27 19:32:58
tags:
    - springcloud
    - springboot
    - java
    - maven
categories: 
    - java
---

* [Hystrix熔断器](#hystrix)
* [Feign实现服务间的调用](#feign)

# <h2 id="hystrix">Hystrix熔断器</h2>

1.在消费者模块添加依赖

``` xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

2.在Application启动程序添加注解 @SpringCloudApplication 并在方法添加注解 @LoadBalanced

``` java
@Bean
@LoadBalanced
public RestTemplate restTemplate() {
    return new RestTemplate();
}
```

3.在方法上添加注解单独使用 @HystrixCommand(fallbackMethod = "") 或者在 controller 类添加 @DefaultProperties(defaultFallback = "") 应用到全部方法。fallback 的方法的参数和返回值必须和原来的方法一致。

# <h2 id="feign">Feign实现服务间的调用</h2>

1.在消费者模块添加依赖

``` xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

2.在Application启动程序添加注解 @EnableFeignClients

3.在消费者模块编写 Feign 接口

``` java
// 提供接口的应用名
@FeignClient("provider-name")
public interface ConsumerClient {
    // 是需要调用的生产者的方法和路径
    @RequestMapping("provider/user")
    User getUser();
}
```

4.直接通过接口调用

``` java
@Autowired
private ConsumerClient consumerClient;

providerClient.getUser();
```
