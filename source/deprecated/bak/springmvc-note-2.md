---
title: SpringMVC 学习笔记(2)
date: 2018-11-09 02:14:44
tags:
    - springmvc
    - java
categories: 
    - note
---

* [Restful 风格](#restful)
* [POST 请求乱码](#messy)
* [拦截器](#interceptor)
* [日期转换器](#date)
* [GET 请求乱码](#get_messy)
* [异常处理](#exception)
* [图片上传](#image)
* [JSON 交互](#json)
* [PUT, DELETE 方法支持](#put_del)

### <h2 id="restful">Restful 风格</h2>

``` python
@RequestMapping("test/{id}")
// public String test(@PathVariable("id") Integer integer) {}
public String test(@PathVariable Integer id) {}
```

### <h2 id="messy">POST 请求乱码</h2>

``` python
<!-- 通过过滤器自动设置 POST 请求编码 -->
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <!-- 设置编码参是UTF8 -->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### <h2 id="interceptor">拦截器</h2>

``` python
// 编写拦截器
public class MyInterceptor implements HandlerInterceptor {

    // 方法执行之后执行, 处理异常, 清理资源, 记录日志
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)throws Exception {}

    // 方法执行之后, 返回 ModelAndView 前执行。设置页面的共同参数
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,ModelAndView modelAndView) throws Exception {}

    // 进入方法前执行, 登陆拦截, 权限校验
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)throws Exception {}
}

// 在 SpringMVC 配置文件中配置
<mvc:interceptors>
    <mvc:interceptor>
        <!-- /** 拦截所有请求, 包括二级目录 -->
        <mvc:mapping path="/**" />
        <!-- 配置不拦截目录 -->
        <mvc:exclude-mapping path="" />
        <bean class="com.uerax.springmvc.interceptor.MyInterceptor" />
    </mvc:interceptor>
    <!-- 若添加多个拦截器则 preHandle 方法按照堆顺序先入先执行, postHandle 和 afterCompletion 按照出栈先入后执行 -->
    <mvc:interceptor>
        <!-- /** 拦截所有请求, 包括二级目录 -->
        <mvc:mapping path="/**"/>
        <bean class="com.uerax.springmvc.interceptor.MyInterceptor2" />
    </mvc:interceptor>
</mvc:interceptors>
```

### <h2 id="date">日期转换器</h2>

``` python
// 编写日期转换器实现 Converter 接口
public class DateConvert implements Converter<String, Date> {

    public Date convert(String source) {
        Date date = null;
        try {
            // 与 JSP 传入的格式一致
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            date = sdf.parse(source);
        } catch (ParseException e) {
            e.printStackTrack();
        }
        return date;
    }
}
// Spring 配置
<!-- 使用自定义转换器 -->
<mvc:annotation-driven conversion-service="MyConvert" />
<!-- 定义转换器 -->
<bean id="MyConvert" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="package.DateConvert" />
        </set>
    </property>
</bean>
```

### <h2 id="get_messy">GET 请求乱码</h2>

* 把 tomcat 配置文件 server.xml 的 <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/> 添加上 URIEncoding="utf-8" 属性

### <h2 id="exception">异常处理</h2>

``` python
// 编写全局异常处理器
public class CustomerExceptionResolver implements HandlerExceptionResolver {
    public ModelAndView resolveException(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2,Exception arg3) {
        // 系统出现异常则返回该试图
        ModelAndView mav = new ModelAndView();
        return mav;
    }
}
// 配置 SpringMVC
<bean class="com.uerax.springmvc.exception.CustomerExceptionResolver" />
```

### <h2 id="image">图片上传</h2>

* 需要导入的 jar 包 : fileupload 和 io 包

``` python
// 在 tomcat 的 server.xml 文件中配置
// docBase -> 文件所在的目录  path -> 配置访问路径
<Context docBase="url" path="/pic" reloadable="true" />

// 在 SpringMVC 的配置文件中配置多媒体处理器
<bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 最大上传文件, value 用字节计算-->
    <property name="maxUploadSize" value="" />
</bean>

// 控制器获取图片参数, 表单必须添加 enctype="multipart/form-data", 必须使用 POST 方法
<form action="" enctype="multipart/form-data" method="POST"></form>
public String test(MultipartFile pictureFile) {
    // 图片新名称
    String newName = UUID.randomUUID().toString();
    // 图片旧名称
    String oldName = pictureFile.getOriginalFilename();
    // 获取图片后缀
    String sux = oldName.subString(oldName.lastIndexOf("."));
    File file = new File("url" + newName + sux);
    // 写入磁盘
    pictureFile.transferTo(file);
}
```

### <h2 id="json">JSON 交互</h2>

* 需要导入的 jar 包 : jackson-annotations, jackson-core, jackson-databind

``` python
@RequestMapping("test")
public @ResponseBody Student test(@RequestBody Student student) {
    return student;
}
```

### <h2 id="put_del">PUT, DELETE 方法支持</h2>

``` python
// 对于表单提交，tomcat默认只解析POST的表单，对于PUT和DELETE的不处理
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<filter>
    <filter-name>HttpPutFormContentFilter</filter-name>
    <filter-class>org.springframework.web.filter.HttpPutFormContentFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HttpPutFormContentFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

#### 通过修改 tomcat 的 server.xml 配置文件

``` python
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443"
           parseBodyMethods="POST,PUT,DELETE"
           URIEncoding="UTF-8" />
```