---
title: SpringBoot 学习笔记二
date: 2019-04-23 21:10:35
tags: 
    - springboot
    - java
    - mybatis
categories: 
    - java
---

* [Thymeleaf](#thymeleaf)
* [JDBC配置](#jdbc)
* [Mybatis](#mybatis)

### <h2 id="thymeleaf">Thymeleaf</h2>

``` python

// 配置 application.yml
Spring:
    thymeleaf:
        # 配置视图路径前缀
        prefix: classpath:/templates/
        # 配置视图路径后缀
        suffix: .html
        mode: html
        # 关闭缓存 修改视图 刷新浏览器就显示 开发阶段务必关闭缓存 (=false)
        cache: false
```

