---
title: JavaWeb 笔记(补)
date: 2018-11-01 22:07:32
tags:
    - servlet
    - java
    - jdbc
    - jsp
categories: 
    - note
---

* [Servlet](#servlet)
* [Filter/Listener](#fal)
* [JSP](#jsp)
* [JDBC](#jdbc)

### <h2 id="servlet">Servlet 配置</h2>

``` python
<servlet>
    <servlet-name>ServletName</servlet-name>
    <servlet-class>package.ServletName</servlet-class>
    // 可选, 使 Servlet 在服务器启动时就创建, 根据 n(n > 0) 的值从小到大开始
    <load-on-startup>n</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>ServletName</servlet-name>
    <url-pattern>/urlName</url-pattern>
</servlet-mapping>

// 表单传递
<form action="urlName">
    <input type="text" name="valueName">
</form>
```

### Servlet Config

``` python
// 获取表单参数
String str = request.getParameter("valueName");

// 参数放入暂存区
request.setAttribute("number", str);

// 从暂存区取出参数
String str = (String) request.getAttribute("number");
// 从暂存区删除删除
request.removeAttribute("number");

// 配置文件配置公共参数, 定义的参数在 JSP 中也能调用
<web-app>
    <context-param>
        <param-name>name</param-name>
        <param-value>value</param-value>
    </context-param>
</web-app>

// 获取公共参数
String value = getInitParameter("name");

// 跳转到 servletName , 地址栏中的地址不变
request.getRequestDispatcher("servletName").forward(request, response);
// 运行(不跳转) servletName 后继续运行原 Servlet
request.getRequestDispatcher("servletName").include(request, response);
// 重定向到 servletName , 地址栏中的地址改变
request.sendRedirect("servletName");
```

### <h2 id="fal">Filter / Listener</h2>

``` python
// 配置文件配置参数
<filter>
    <filter-name>name</filter-name>
    <filter-class>package.filterClass</filter-class>
</filter>
<filter-mapping>
    <filter-name>name</filter-name>
    // 过滤器作用的对象
    <url-pattern>*.jsp</url-pattern>
</filter-mapping>

<listener>
    <listener-class>package.listener</listener-class>
</listener>
```

### <h2 id="jsp">JSP</h2>

``` python
// JSP 指令 <%@ 指令名 key1="value1" key2="value2" %>
<%@ page language="java" import="java.util.*, java.lang.*" pageEncoding="utf-8" %>

// JSP 代码段
// 存在 Servlet 的 Service 方法内, 每次刷新页面重新定义
// 不能定义方法
<% int i = 1; %>

// JSP 表达式
<%= i %>

// JSP 定义区
// 存在 Servlet 内作为其属性, 刷新页面不会重新定义
// 可以定义方法
<%! int i = 2; %>

// JSP 注释
<%--  --%>
```

### JSTL 标签库

* 需要的 jar 包 : JSTL 包

``` python
// jsp 文件导入 jstl 标签库
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

// c:forEach
// items 集合对象, var 每一次值存放的变量
<c:forEach items="${}" var="${var}" begin="" end="">
    <c:out value="${var}" />
</c:forEach>

// c:forTokens
// delims -> 分隔符
<c:forTokens items="" delims="" var="${var}">
    <c:out value="${var}" />
</c:forTokens>

// c:set 存放数据
<c:set var="key" value="value">
// 存放 Map 及对象, target 是 Map 则 property 指定的是 key, 是对象则 property 指定的是对象的属性
<c:set target="${}" property="name" value="value" />

// c:remove
<c:remove var="">

// c:out 类似 <%= %>
<c:out value="${key}" />

// c:catch 获取错误信息并存放到变量里
<c:catch var="">Exception</c:catch>

// c:if 即 if 语句 test -> 条件
<c:if test="${}"></c:if>

// c:choose 类似 switch 语句, c:when 类似 case, c:otherwise 类似 default
<c:choose>
    <c:when test="">
    </c:when>
    <c:otherwise>
    </c:otherwise>
</c:choose>

// c:redirect 重定向
<c:redirect url=""/>

// c:url 将 URL 地址格式化为一个字符串
<c:url var="" url=""/>


```

### <h2 id="jdbc">JDBC</h2>

``` python
private String JDBC_URL = "jdbc:mysql:///database_name";
private String USERNAME = "username";
private String PASSWORD = "password";
// 使用 ? 作为占位符
private String SQL = "select * from table where id = ?" 

// 检查驱动是否存在
Class.forName("com.mysql.jdbc.Driver");
// 获取连接
Connection conn = DriverManager.gerConnection(JDBC_URL, USERNAME, PASSWORD;
// 读取 SQL 语句 
PreparedStatement ps = conn.prepareStatement(SQL);
// 按输入的数据类型选择方法, 1 代表第一个 ？
ps.setString(1, "test");

// 增删改
ps.executeUpdate();

// 查
// 使用 ResultSet 保存取出来的数据
ResultSet rs = ps.executeQuery();
// 遍历 ResultSet 获取数据, 每次一条
while (rs.next()) {
    int sqlCol = rs.setInt("sql_col");
    String sqlColName = rs.setString("sql_col_name");
}
```