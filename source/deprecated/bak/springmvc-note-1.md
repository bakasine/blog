---
title: SpringMVC 学习笔记(1)
date: 2018-11-05 19:42:15
tags:
    - springmvc
    - java
    - ssm
categories: 
    - note
---

* [配置环境](#environment)
* [SSM 框架整合](#ssm)

### <h2 id="environment">配置环境</h2>

* 需要的 jar 包 : Spring 基础 jar 包, Spring 的 aop, web, webmvc 包, JSP 标签库 jstl 包, 日志 commons-logging 包

### 基础流程

``` python
// 创建 controller 控制器
@Controller
public class TestController {
    // 通过 ModelAndView返回
    @RequestMapping("test")
    public ModelAndView test() {
        ModelAndView mav = new ModeAndView();
        // 设置模型数据传递到 jsp, key = "msg", value="test"
        // jsp 文件获取数据 ${ key }
        mav.addObject("msg", "test");
        // 设置 jsp 视图
        mav.setViewName("path/jsp.jsp");
        return mav;
    }

    // 通过字符串返回视图名称
    @RequestMapping("test")
    public String test(Model model) {
        model.addAttribute("", "");
        return "test";
    }
    // 配置多个请求地址, 设置提交方法限定
    @RequestMapping(value={"test", "test2"}, method=RequestRequest.POST)

    // 通过传入参数获取数据, integer 必须和传入的参数名称相同
    public String test(Model model, Integer integer) {}

    // 如果要使 integer 和传入参数名称不同
    public String test(Model model, @RequestParam("parameter") Integer integer) {}
    // required 要求参数必须传入否则会报错, defaultValue 设置默认参数
    public String test(Model model, @RequestParam(value="parameter", required=true, defaultValue="") Integer integer) {}

    // 传递数组参数
    public String test(Integer[] integer) {}

    // 通过返回值跳转
    public String test() {
        return "forword:url";
        // return "redirect:url";
    }

    // 传入实体类参数, 要求传入的值的名称要和实体类的属性名一致
    public String test(Student stu) {}

    // 传入包装实体类
    public String test(Human human, Model model) {}
    // Human 内有 private Student student;
    <input type="" name="student.name" />

    // 传递　List 参数
    <c:forEach items="" var="var" varStatus="status">
        <input type="text" name="var[${status.index}].name" value="${var.name}" />
    </c:forEach>
    public class Human { private List<Student> lists; }
    public String test(Human human) {}


}

// 配置 Spring 配置文件 springmvc.xml
// 需要扫描的包
<context:component-scan base-package="" />

<!-- 配置注解驱动, 相当于同时使用最新处理器映射器和处理器适配器, 对 json 数据提供支持  -->
<mvc:annotation-driven />

<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/" />
    <property name="suffix" value=".jsp" />
    <!-- ModelAndView.setViewName("jsp") 内的 jsp = /WEB-INF/jsp/jsp.jsp -->
</bean>

<!-- 将 SpringMVC 不能处理的请求交给 Tomcat  -->
<mvc:default-servlet-handler/>

// 配置 web.xml 文件
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-value>org.springframework.web.servlet.DispatcherServlet</servlet-value>
    // 如果不配置这段那么 springmvc.xml 的名称需要修改成 dispatcherServlet-servlet.xml 并放在与 web.xml文件同级目录下
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
</servlet>

<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    // 给 springmvc 处理器配置拦截地址
    <url-param>*.action</url-param>
</servlet-mapping>
```

### <h2 id="ssm">SSM 框架整合</h2>

``` python
// 创建 MyBatis 核心配置文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration></configuration>

// 创建 Spring 配置文件
// applicationContext-dao.xml 持久层配置
<context:property-placeholder location="db.properties"/>
// 配置数据库连接池
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="${db.dirver}" />
    <property name="url" value="${db.url}" />
    <property name="username" value="${db.user}" />
    <property name="password" value="${db.pw}" />
</bean>
// 配置 SqlSessionFactory
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="configLocation" value="classpath:SqlMapConfig.xml" />
    // 如果要让 Mapper 的 xml 文件和接口放在不同位置则需要添加
    <property name="mapperLocations" value="classpath:mapper/*.xml" />
</bean>
// 配置 MyBatis 映射扫描
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.uerax.springmvc.mapper" />
</bean>

// applicationContext-service.xml serice 层配置
<context:component-scan base-package="com.uerax.springmvc.service" />

// applicationContext-transaction.xml 业务配置
<!-- 事务管理器配置 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource" />
</bean>
<!-- 配置通知 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="save*" propagation="REQUIRED" />
        <tx:method name="insert*" propagation="REQUIRED" />
        <tx:method name="delete*" propagation="REQUIRED" />
        <tx:method name="update*" propagation="REQUIRED" />
        <tx:method name="find*" propagation="SUPPORTS"
            read-only="true" />
        <tx:method name="get*" propagation="SUPPORTS"
            read-only="true" />
        <tx:method name="query*" propagation="SUPPORTS"
            read-only="true" />
    </tx:attributes>
</tx:advice>
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.uerax.springmvc.*.*(..))"/>
</aop:config>

// 在 web.xml 配置
<!-- Spring 的配置文件 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>applicationContext*.xml</param-value>
</context-param>
<!-- 自动加载 Spring 的配置文件 -->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```