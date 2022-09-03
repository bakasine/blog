---
title: SpringBoot 学习笔记一
date: 2019-04-18 22:07:02
tags: 
    - spring boot
    - java
    - mybatis
categories: 
    - java
---

* [注释](#annotations)
* [JDBC配置](#jdbc)
* [Mybatis](#mybatis)

### <h2 id="annotations">注释</h2>

``` python
@SpringBootApplication :
复合注解，包括 @ComponentScan，和 @SpringBootConfiguration，@EnableAutoConfiguration。
@Configuration :
声明该类是为类似 spring 的 XML 配置文件
@Bean :
等价于 XML 中配置的 bean
@Value :
注入 Spring boot application.properties 配置的属性的值,通过${}获取
@PropertySource :
加载指定的 properties 文件
@ConfigurationProperties :
将指定的前缀的数据封装到实体类相同名称的属性内，即不需要在每个属性都添加 @Value。如果报错可以添加 @Component 注解。需要设置 Getter 和 Setter
@EnableConfigurationProperties :
将指定的实体类作为配置信息
```

### <h2 id="jdbc">JDBC配置</h2>

``` python
方法一 :
@ConfigurationProperties(prefix = "")
public class JdbcProperties {
    private String url;
    public String getUrl() {
        return this.url;
    }
    public void setUrl(String url) {
        this.url = url;
    }
}

@Configuration
@EnableConfigurationProperties(JdbcProperties.class)
public class JdbcConfig {
    @Bean
    public DataSource dataSource(JdbcProperties prop) {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(prop.getDriverClassName());
        dataSource.setUrl(prop.getUrl());
        dataSource.setUsername(prop.getUsername());
        dataSource.setPassword(prop.getPassword());
        return dataSource;
    }
}

方法二 :
@Configuration
@EnableConfigurationProperties(JdbcProperties.class)
public class JdbcConfig {

    @Autowired
    private JdbcProperties prop;

    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(prop.getDriverClassName());
        dataSource.setUrl(prop.getUrl());
        dataSource.setUsername(prop.getUsername());
        dataSource.setPassword(prop.getPassword());
        return dataSource;
    }
}

方法三 :
@Configuration
public class JdbcConfig {
    @Bean
    @ConfigurationProperties(prefix = "")
    public DataSource dataSource() {
        // application.properties 文件的名称必须为 driverClassName,url,username,password
        DruidDataSource dataSource = new DruidDataSource();
        return dataSource;
    }
}
```

### <h2 id="mybatis">整合MyBatis</h2>

``` python

// 事先准备 User 实体类(Getter，Setter)，user 数据库表

// 创建 MyBatis 的映射

一.注解法

@Mapper
public interface UserMapper {
    @Select("select * from user where username = #{username}")
    User findUserByUsername(@Param("username") String username);
}

二.XML配置法
// 需要在 application.yml 额外添加 xml 文件的路径, xml 文件编写见 Mybatis
mybatis:
  type-aliases-package: com.uerax.springboot.domain
  mapper-locations: mapper/*.xml

// UserMapper.java
@Mapper
public interface UserMapper { 
    User findUserById(@Param("id") int id);
}

// UserMapper.xml 编写
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.uerax.springboot.mapper.UserMapper">
    <select id="findUserById" parameterType="int" resultType="User">
      select * from user where id = #{id}
    </select>
</mapper>

// 配置 application.yml 的 DataSource
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    username: root
    password: root
    url: jdbc:mysql:///springboot?serverTimezone=GMT
// 因为 SpringBoot 使用的是最新 mysql-connector-java 8+ ，JDBC 连接到mysql-connector-java 6+以上的需要指定时区 serverTimezone=GMT%2B8
```
