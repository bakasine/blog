---
title: SpringCloud 笔记一
date: 2020-03-26 18:38:50
tags:
    - springcloud
    - springboot
    - java
    - maven
categories: 
    - java
---

* [Maven聚合工程](#maven)
* [Eureka配置](#eureka)
* [Eureka配置Client](#client)

# <h2 id="maven">Maven聚合工程</h2>

1.spring initializr 或者 maven 创建一个父亲工程

2.spring initializr 创建需要将 Type 改成 maven pom, maven 需要添加 packaging 标签为 pom

3.引入依赖

``` xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/>
</parent>
<properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>Hoxton.SR3</spring-cloud.version>
</properties>
<!-- dependencyManagement 子项目需要引用才有效 -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<!-- dependencies 子项目无需引用就有效 -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

# <h2 id="eureka">Eureka配置Server</h2>

1.添加 Eureka 子工程，添加依赖

``` xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

2.在Application启动程序添加注解 @EnableEurekaServer 和配置 Eureka 参数

``` yml
server:
  port: 8080
spring:
  application:
    name: eureka-server
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:8080/eureka
```

# <h2 id="client">Eureka配置Client</h2>

1.新增一个生产者子工程并添加依赖

``` xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2.在Application启动程序添加注解 @EnableDiscoveryClient
和配置 Eureka 参数

``` yml
server:
  port: 8088
spring:
  application:
    name: eureka-producer
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:8080/eureka
```

3.同样的参数新增一个消费者子工程，消费的方法如下

``` java
// Spring 提供的用于访问 Rest 服务的客户端
@Autowired
private RestTemplate restTemplate;

// 通过 Eureka 让服务器发现服务器
@Autowired
private DiscoveryClient discoveryClient;

@RequestMapping("")
public User getUser() {
    // 生产者的spring.application.name
    List<ServiceInstance> list = discoveryClient.getInstances("service-web");
    ServiceInstance instance = list.get(0);
    User user = restTemplate.getForObject("http://" + instance.getHost() + ":" + instance.getPort() + "/", User.class);
    return user;
}
```