---
title: Spring 学习笔记(2)
date: 2018-10-28 16:18:54
tags:
    - java
    - spring
categories: 
    - note
---

* [JDBC](#jdbc)
    * [DBCP](#dbcp)
    * [C3P0](#c3p0)
* [事务管理](#tx)

### <h2 id="jdbc">Spring JDBC</h2>

* 需要引入的 jar 包 : Spring 的 JDBC , tx , aop 的包和数据库驱动的包

``` python
// 将连接池和模板交给 Spring 管理
// 配置连接池
<bean id="dataSourceId" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    // 配置数据库驱动
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    // 配置数据库 database
    <property name="url" value="jdbc:mysql:///database" />
    <property name="username" value="username" />
    <property name="password" value="password" />
</bean>
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSourceId" />
</bean>

// 增 删 改 使用 update方法
jdbcTemplate.update("sql", args);

// 查 使用 queryForObject方法
// 返回 String 类型的值
String str = jdbcTemplate.queryForObject("sql", String.class, args)
```

#### <h2 id="dbcp">使用开源连接池 DBCP 连接池</h2>

* 需要引入的 jar 包 : apache 的 dbcp , pool 包

``` python
// 只需要将配置连接池的 class 改成 org.apache.commons.dbcp2.BasicDataSource 其他用法不变
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    <property name="url" value="jdbc:mysql:///spring" />
    <property name="username" value="root" />
    <property name="password" value="root" />
</bean>
```

#### <h2 id="c3p0">使用 C3P0 连接池</h2>

* 需要引入的 jar 包 : c3p0 的 c3p0 , mchange-commons 包

``` python
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver" />
    <property name="jdbcUrl" value="jdbc:mysql:///spring" />
    <property name="user" value="root" />
    <property name="password" value="root" />
</bean>
```

### 通过 properties 文件保存

``` python
// 创建 properties 文件, 语法为 key=value
driverClass=com.mysql.jdbc.Driver

// 需要在配置文件配置
<context:property-placeholder location="classpath:file.properties">

// 通过 ${} 调用
<property name="" value="${key}">
```

### <h2 id="tx">Spring 事务管理</h2>

#### 声明式事务

* 通过配置实现
* 需要引入的 jar 包 : AOP 所需要的全部包

#### XML 配置方法

``` python
// 配置平台管理器
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    // 配置数据库连接池
    <property name="dataSource" ref="dataSource" />
</bean>

// 配置增强
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="methodName" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>

// AOP 配置
<aop:config>
    <aop:pointcut expression="execution(* com.uerax.spring.tx.demo1.StudentServiceImpl.*(..))" id="pointcut1" />
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1" />
</aop:config>
```

#### 注解方法

``` python
// 配置平台管理器
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    // 配置数据库连接池
    <property name="dataSource" ref="dataSource" />
</bean>

// 开启注解事务
<tx:annotation-driven transaction-manager="transactionManager"/>

// 在业务层添加注解
@Transactional
public class Service {}
```

### 编程式事务

* 需要手动书写代码

``` python
// 配置平台管理器
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    // 配置数据库连接池
    <property name="dataSource" ref="dataSource" />
</bean>

// 配置事务管理模板
<bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
    <property name="transactionManager" ref="transactionManager" />
</bean>

// 在需要添加事务的方法中添加
transactionTemplate.execute(new TransactionCallbackWithoutResult() {
    protected void doInTransactionWithoutResult(TransactionStatus arg0) {
        // 需要添加事务的方法
    }});
```