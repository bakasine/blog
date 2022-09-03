---
title: thymeleaf
date: 2018-12-12 16:14:24
tags:
    - java
categories: 
    - note
---

* [目录](#none)
    * [配置](#environment)
    * [表达式语法](#expression)
    * [常用标签](#tag)
        * [th:text](#text)
        * [th:utext](#utext)
        * [th:if](#if)
        * [th:unless](#unless)
        * [th:switch](#switch)
        * [th:each](#each)
        * [th:selected](#selected)
        * [th:value](#value)
        * [th:action](#action)
        * [th:src](#src)
        * [th:href](#href)
    * [内置对象](#object)
    * [使用笔记](#note)

### <h2 id="environment">SpringMVC 配置文件下配置 Thymeleaf</h2>

* 依赖 : thymeleaf-spring5, spring-context-support

``` python
<bean id="templateResolver" class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
    <property name="prefix" value="/WEB-INF/views/" />
    <property name="suffix" value=".html" />
    <property name="templateMode" value="HTML5" />
    <property name="characterEncoding" value="UTF-8" />
</bean>

<bean id="templateEngine" class="org.thymeleaf.spring5.SpringTemplateEngine">
    <property name="templateResolver" ref="templateResolver" />
</bean>

<-- 配置 Thymeleaf 视图解析器替代 InternalResourceViewResovler -->
<bean class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="templateEngine" ref="templateEngine" />
    <-- 解决中文乱码问题 -->
    <property name="characterEncoding" value="UTF-8" />
</bean>

// 在 html 标签引入
<html xmlns:th=“http://www.thymeleaf.org">
```

### <h2 id="expression">表达式语法</h2>

* ${} : 变量表达式

``` python
<p><span th:text="${helloword}"></span></p>
```

* *{} : 选择表达式

``` python
<-- 选择表达式 th:object 对象绑定的属性, 如果没有选择对象则和变量表达式语法一致 -->
<div th:object=" ${session.user}" >
    <p>Name: <span th: text=" *{firstName}" >Sebastian</span>. </p>
    <p>Surname: <span th: text=" *{lastName}" >Pepper</span>. </p>
    <p>Nationality: <span th: text=" *{nationality}" >Saturn</span>. </p>
</div>
```

* @{} : 超链接url表达式

``` python
<script th:src="@{/resources/js/jquery/jquery.json-2.4.min.js}"
```

* #{} : 消息表达式

``` python
<-- 通常与 th:text 一起使用, 使用消息表达式内的 key 所对应的 value 代替标签内文本 -->
<p th:text="#{home.welcome}" >This text will not be show! </p>
```

### <h2 id="tag">常用标签</h2>

* <h3 id="text">th:text</h3> : 用于文本的显示

``` python
<p th:text=""></p>
```

* <h3 id="utext">th:utext</h3> : 和 th:text 类似但可以解析 HTML 文本

``` python
<p th:utext=""></p>
```

* <h3 id="if">th:if</h3> : 用于判断条件, 可以与 and, or, !, not 一同使用, 如果条件为 true 则显示

``` python
<div th:if="${a} != null"></div>
```

* <h3 id="unless">th:unless</h3> : 与 th:if 作用相反, 如果条件为 false 则显示

``` python
<div th:if="${user} != null">show</div>
```

* <h3 id="switch">th:switch th</h3>:case

``` python
<div th:switch="${user.role}">
    <p th:case="'admin'">User is an administrator</p>
    <p th:case="#{roles.manager}">User is a manager</p>
</div>
```

* <h3 id="each">th:each : foreach</h3>

``` python
<tr th:each="prod : ${prods}">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod}">arrays</td>
</tr>
```

* <h3 id="selected">th:selected</h3> : 选中 select 的选项

``` python
<option th:if="${}" th:selected="selected"></option>
```

* <h3 id="value">th:value</h3> : 属性赋值, 用于 option 和 input

``` python
<option th:value="${user.name}"></option>
```

* <h3 id="action">th:action</h3> : 定义后台控制器的路径

``` python
<form th:action="@{user/login}" method="post"></form>
```

* <h3 id="src">th:src</h3> : 外部资源引入

``` python
<img th:src="@{../images/myself.jpg}"/>
<script th:src="@{../static/login.js}"></script>
```

* <h3 id="href">th:href</h3> : 定义超链接

``` python
<a th:href="@{/user}"></a>
```

* th:remove : 用于删除
    * all : 删除所在标签及其全部内容
    * body : 不删除标签, 但删除子标签和其内容
    * tag : 删除所在标签, 不删除子标签
    * all-but-first : 删除除了第一个子标签以外的其他子标签
    * none : 什么都不删

``` python
<tbody th:remove="${choice}? all : all-but-first"><tbody>
```

* th:object : 对象绑定

``` python
<form th:object="${user}">
    <input th:value="*{name}" />
</form>
```

### <h2 id="object">内置对象</h2>

``` python
#dates : 日期格式化内置对象, 具体方法可以参照java.util.Date;
#calendars : 类似于#dates, 但是是java.util.Calendar类的方法;
#numbers： : 数字格式化;
#strings : 字符串格式化, 具体方法可以参照java.lang.String, 如startsWith、contains等;
#objects : 参照java.lang.Object;
#bools : 判断boolean类型的工具;
#arrays : 数组操作的工具;
#lists : 列表操作的工具, 参照java.util.List;
#sets : Set操作工具, 参照java.util.Set;
#maps : Map操作工具, 参照java.util.Map;
#aggregates : 操作数组或集合的工具;
#messages : 操作消息的工具;
```

### <h2 id="note">使用笔记</h2>

``` python
// 获取项目路径
<meta name="ctx" th:content="${#httpServletRequest.getContextPath()}" />
var _ctx = $("meta[name='ctx']").attr("content");


```