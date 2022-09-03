---
title: MyBatis 分页插件
date: 2018-11-12 15:07:52
tags:
    - mybatis
    - java
categories: 
    - note
---

* 需要引入的 jar 包 : pagehelper 包, sql 解析器的 sqlparser 包
* 使用 maven 只需要添加 pagehelper 依赖

### 配置

#### 方法一

``` python
// 使用 MyBatis 配置文件配置
<plugins>
    <!-- com.github.pagehelper为PageHelper类所在包名 -->
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
        <!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->
        <property name="param1" value="value1"/>
    </plugin>
</plugins>
```

#### 方法二

``` python
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <!-- 注意其他配置 -->
  <property name="plugins">
    <array>
      <bean class="com.github.pagehelper.PageInterceptor">
        <property name="properties">
          <!--使用下面的方式配置参数，一行配置一个 -->
          <value>
            params=value1
          </value>
        </property>
      </bean>
    </array>
  </property>
</bean>
```

### 参数

``` python
// reasonable：分页合理化参数，默认值为false。当该参数设置为 true 时，pageNum<=0 时会查询第一页， pageNum>pages（超过总数时），会查询最后一页。默认false 时，直接根据参数进行查询。
<property name="reasonable" value="true"/>

// offsetAsPageNum：默认值为 false，该参数对使用 RowBounds 作为分页参数时有效。 当该参数设置为 true 时，会将 RowBounds 中的 offset 参数当成 pageNum 使用，可以用页码和页面大小两个参数进行分页。
<property name="reasonable" value="true"/>
```

### 使用方法

``` python

// 只有紧跟在PageHelper.startPage方法后的第一个Mybatis的查询（Select）方法会被分页。

public String getEmp(Model model, @RequestParam(defaultValue="1") Integer startPage) {
    // 获取第1页，10条内容，默认查询总数count
    PageHelper.startPage(startPage, 5);
    List<Employee> list = employeeService.getEmployee();
    // 将 list 放进 PageInfo 并设置显示页码数如不设置默认为8
    PageInfo page = new PageInfo(list, 5);
    model.addAttribute("pageInfo", page);
    return "list";
}

// pageContext.setAttribute("APP_PATH", request.getContextPath());
// 获取 PageInfo 内的 list 遍历
<c:forEach items="${ pageInfo.list }" var="emp"></c:forEach>
// 修改 bootstrap 默认的分页模板
<div class="row">
    <div class="col-md-3 col-md-offset-8">
        <nav aria-label="Page navigation">
            <ul class="pagination">
                <li><a href="${ APP_PATH }/emps?startPage=${ pageInfo.pageNum - 1 }" aria-label="Previous"> <span
                        aria-hidden="true">&laquo;</span>
                </a></li>
                <!-- 获取所有导航页码遍历 -->
                <c:forEach items="${ pageInfo.navigatepageNums }" var="page">
                    <!-- 判断是否为当前页码 -->
                    <c:if test="${ page == pageInfo.pageNum }">
                        <li class="active"><a href="#">${ page }</a></li>
                    </c:if>
                    <c:if test="${page != pageInfo.pageNum }">
                        <li><a href="${ APP_PATH }/emps?startPage=${page}">${ page }</a></li>
                    </c:if>
                </c:forEach>
                <li><a href="${ APP_PATH }/emps?startPage=${ pageInfo.pageNum + 1 }" aria-label="Next"> <span
                        aria-hidden="true">&raquo;</span>
                </a></li>
            </ul>
        </nav>
    </div>
</div>
```