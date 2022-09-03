---
title: MyBatis 整合 Spring 框架
date: 2018-11-03 20:54:58
tags:
    - spring
    - mybatis
    - java
categories: 
    - note
---

### 整合环境

* mybatis 的 jar 包
* spring 的 jar 包
* 数据库连接池 jar 包(dbcp连接池)
* 数据库驱动

### 配置文件

#### Spring 配置文件

``` python
<context:property-placeholder location="jdbc.properties"/>
<!-- 配置数据库连接池 -->
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="${jdbc.dirver}" />
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>

<!-- SqlSessionFactory 配置 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!-- 加载数据库连接池 -->
    <property name="dataSource" ref="dataSource" />
    <!-- 加载 MyBatis 核心配置文件 -->
    <property name="configLocation" value="classpath:SqlMapConfig.xml" />
    <!-- 别名包扫描 -->
    <property name="typeAliasesPackage" value="" />
</bean>
```

### Mapper 编写三种方法

#### 原始 DAO 开发

``` python
// mapper.xml
<mapper namespace="">
    <select id="method" parameterType="" resultType="">
        // sql
    </select>
</mapper>

// MyBatis 核心配置文件
<configuration>
    <mappers>
        <mapper resource="mapper.xml" />
    </mappers>
</configuration>

// 编写 DAO 接口及其实现类
public interface StudentDao {}
// 需要继承 SqlSessionDaoSupport
public class StudentDaoImpl extends SqlSessionDaoSupport implements StudentDao {
    public void method() {
        // 通过 SqlSessionDaoSupport 获取 SqlSession
        SqlSession ss = this.getSqlSession();
    }
}

// 需要在 Spring 配置文件中配置
<bean id="studentDao" class="">
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
</bean>

// 测试程序
public class Tester {

    private ApplicationContext applicationContext;

    @Test
    public void test1() {
        applicationContext = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
        // StudentDao stu = (StudentDao) applicationContext.getBean("studentDao");
        StudentDao stu = applicationContext.getBean(StudentDao.class);
    }
}
```

#### Mapper 代理开发

``` python
// 使用mapper代理开发时，namespace有特殊作用，namespace等于mapper接口地址
<mapper namespace="package.StudentMapper">
    <select id="">
        // sql
    </select>
</mapper>

// Spring 配置文件中配置
<bean id="studentMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="sqlSessionFactory" ref="sqlSessionFactory" />
    <property name="mapperInterface" value="package.MapperInterface" />
</bean>

// 测试文件
StudentMapper stuMap = applicationContext.getBean(StudentMapper.class);
```

#### Mapper 动态代理

``` python
// 只需在 Spring 配置文件下添加
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    // 扫描的包路径
    <property name="basePackage" value="com.uerax.mybatis.mapper" />
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
</bean>
```