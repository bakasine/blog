---
title: Spring 学习笔记(1)
date: 2018-10-27 16:42:17
tags: 
    - java
    - spring
categories: 
    - note
---

* [环境配置](#environment)
* [bean](#bean)
* [注释方法](#annotation)
* [AOP](#aop)

### <h2 id="environment">环境配置</h2>

* 基础 jar 包 : Spring 自带的 beans, core, context, expression 包另外还需要 log4j 和 commons-logging 两个日志包

### <h2 id="bean">bean 标签来完成实例化</h2>

``` python
// 需要添加 xml 的 bean 约束用 Eclipse 的 Spring tools 插件自动生成
// beanId -> 实例化后的id    classPath -> 类的包路径
<bean id="beanId" class="classPath"><bean/>

// 属性注入 - set 方法注入, 属性必须实现 set 方法
<bean id="beanId" class="classPath">

    // 普通属性注入
    <property name="属性名" value="注入的值" />

    // 对象类型注入
    <property name="属性名" ref="beanId" />

    // SpEL的属性注入
    <property name="属性名" value="{注入的值(非字符)}" />
    <property name="属性名" value="{'字符串或者字符'}" />

    // 集合类型注入
    // 数组和 List 类型
    <property name="属性名">
        <list>
            <value>注入的值</value>
            <value>注入的值</value>
        <list/>
    </property>

    // Set 类型
    <property name="属性名">
        <set>
            <value>注入的值</value>
        </set>
    </property>

    // Map 类型
    <property name="属性名">
        <map>
            <entry key="keyName" value="value">
        </map>
    </property>
<bean/>

// 可导入其他配置文件
<import resource="path">

// 在 applicationContext.xml 文件下配置好后
// ClassPathXmlApplicationContext -> 读取类包同路径下(如同类包在 src 文件夹下)
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("xml文件路径");
// FileSystemXmlApplicationContext -> 读取工程目录下
ApplicationContext applicationContext = new FileSystemXmlApplicationContext("xml文件路径");
ClassName className = (ClassName) applicationContext.getBean("beanId");
```

### <h2 id="annotation">通过注释实现实例化</h2>

``` python
// 需要添加 xml 的 context 约束用 Eclipse 的 Spring tools 插件自动生成
// 需要在配置文件指定扫描的包
<context:component-scan base-package="需要扫描的包及其子包">

// Contorller 和 Service 和 Repository 一样的功能
@Controller("beanId")
public class className {

    // 普通类型使用 Value
    @Value("value")
    private int intValue;

    @Value("{'value'}")
    private String StringValue;

    // 对象类型使用 Resource 或者 Autowired + Qualifier
    @Resource(name="beanId")
    private ClassName className;
}
```

### 使用 Spring 整合 JUnit4 的功能

* 需要引入的 jar 包 : Spring 的 Test 包

``` python
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:配置文件") // 配置文件放在类包同路径下
// 配置文件放在工程目录下 @ContextConfiguration("file:path/配置文件")
public class classTester() {

    @Resource(name="beanId")
    private ClassName className;

    @Test
    public void method() {
        className.method();
    }
}
```

### <h2 id="aop">Spring AOP</h2>

* 需要引入的 jar 包 : Spring 的 aop, aspect 包

#### AOP 的配置方法

``` python

// 在配置文件或者注解配置切面类
<bean id="aspectClass" class="path"><bean/>

// AOP 配置
<aop:config>
    // 配置切点
    // 常见写法
    // execution(public * *(..))                                   所有的public方法  
    // execution(* set*(..))                                       所有set开头的方法  
    // execution(* com.uerax.service.AccountService.*(..))         AccountService类中的所有方法
    // execution(* com.uerax.service.*.*(..))                      com.uerax.service包下所有的方法  
    // execution(* com.uerax.service..*.*(..))                     com.uerax.service包及其子包下所有的方法
    <aop:pointcut expression="execution(* path.className.method(..))" id="切点名" />

    // 配置切面
    <aop:aspect ref="aspectClass">
        // 前置通知 -> 执行前检查
        <aop:before method="aspectClass的方法" pointcut-ref="切点名" />

        // 后置通知 -> 执行后检查
        <aop:after-returning method="" pointcut-ref="" />

        // 环绕通知 -> 执行前后都检查
        <aop:around method="" pointcut-ref="" />

        // 异常抛出通知
        <aop:after-throwing method="" pointcut-ref="" throwing="ex(抛出的异常id)"/>

        // finally 通知 -> 方法报错也会执行
        <aop:after method="" pointcut-ref="" />
    </aop:aspect>
</aop:config>

// 切面类
public class AspectClass {
    // 前置通知
    // joinPoint 连接点，指的是被增强的那个方法
    public void before(JoinPoint joinPoint) {}
  
    // 后置通知
    // result 增强的方法的返回值
    public void afterReturning(JoinPoint joinPoint, Object result) {}

    // 环绕通知
    // proceedingJoinPoint 正在执行的连接点
    public Object around(ProceedingJoinPoint proceedingJoinPoint) {}

    // 异常抛出通知
    // ex 目标方法抛出的异常 要与配置文件命名一致
    public void afterThrowing(JoinPoint joinPoint, Throwable ex) {}

    // 最终通知
    // 作用：不管目标方法是否发生异常，最终通知都会执行 (类似于finally代码功能)
    // 应用场景 : 释放资源 (关闭文件、 关闭数据库连接、 网络连接、 释放内存对象)
    public void after(JoinPoint joinPoint) {}
}
```

#### AOP 的注释方法

``` python
// 需要先在配置文件开启注解的 AOP 开发
<aop:aspectj-autoproxy/>

// 在切面类上添加
@Aspect
public void AspectClass {
    // 前置通知
    @Before("execution(* *.className.method(..))")
    public void before() {}

    // 后置通知
    // Object 声名的变量必须与注释配置的 returning 一致
    @AfterReturning(value="execution(* *.className.method(..))", returning="result")
    public void afterReturning(Object result) {}

    // 环绕通知
    @Around(value="execution(* *.className.method(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {}

    // 异常抛出通知
    @AfterThrowing(value="execution()")

    // 最终通知
    @After(value="AspectClass.pointcut()", throwing="e")

    // 配置切入点
    // 直接用 className.pointcut() 选择该切入点
    @Pointcut("execution()")
    private void pointcut() {}
}
```