---
title: SpringCloud 笔记三
date: 2020-03-28 18:13:35
tags:
    - springcloud
    - springboot
    - java
    - maven
categories: 
    - java
---

* [Zuul路由](#gateway)
* [Zuul拦截器](#filter)

# <h2 id="gateway">Zuul路由</h2>

1.创建 Zuul 的子模块并添加依赖

``` xml
<!-- 使用阿里的maven库下载的包有缺漏导致一直爆红。用原生库问题解决 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>
<!-- 用于 eureka -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2.在Application启动程序添加注解 @EnableZuulProxy 和配置 Zuul 参数

``` yml
# 将 service-provider 应用绑定到以 /provider 为前缀的请求，Zuul回默认绑定绑定应用名为前缀
zuul:
  routes:
    service-provider: /provider/**
  # 不默认绑定 service-test 到 /service-test 上 
  ignoredServices: service-test
```

# <h2 id="filter">Zuul拦截器</h2>

1.创建类并继承 ZuulFilter

``` java
@Component
public class MyFilter extends ZuulFilter {
    @Override
    public String filterType() {
        // pre: 这种过滤器在请求被路由之前调用。可利用这种过滤器实现身份验证、在集群中选择请求的微服务，记录调试信息等。
        // routing: 这种过滤器将请求路由到微服务。这种过滤器用于构建发送给微服务的请求，并使用apache httpclient或netflix ribbon请求微服务。
        // post: 这种过滤器在路由到微服务以后执行。这种过滤器可用来为响应添加标准的http header、收集统计信息和指标、将响应从微服务发送给客户端等。
        // error: 在其他阶段发送错误时执行该过滤器。 
        return FilterConstants.PRE_TYPE;
    }

    @Override
    public int filterOrder() {
        // 通过int值来定义过滤器的执行顺序，越小优先级越高
        return FilterConstants.PRE_DECORATION_FILTER_ORDER - 1;
    }

    @Override
    public boolean shouldFilter() {
        // 判断是否过滤
        return true;
    }

    @Override
    public Object run() throws ZuulException {
        // 过滤器的具体逻辑
        RequestContext context = RequestContext.getCurrentContext();
        HttpServletRequest request = context.getRequest();
        String s = request.getParameter("access-token");

        if (StringUtils.isBlank(s)) {
            context.setSendZuulResponse(false);
            context.setResponseStatusCode(HttpStatus.FORBIDDEN.value());
        }
        return null;
    }
}
```