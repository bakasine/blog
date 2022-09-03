---
title: Maven
date: 2018-11-10 23:52:24
tags:
    - maven
categories: 
    - note
---

### 创建 maven 工程

#### 使用阿里云镜像仓库

``` python
// 在 maven 的 settings.xml 文件的 mirrors 标签下添加
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

#### 指定 maven 默认使用的 JDK 版本

``` python

<!-- 局部jdk配置，pom.xml中 -->
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>

<!-- 全局jdk配置, settings.xml -->
<profile>
    <id>jdk18</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
    </activation>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
</profile>
```

### 引入 jar 包

* maven 仓库 [mvnrespository](http://mvnrepository.com/)

``` python
// 在 pom.xml 文件中的 dependencies 标签内添加
<dependencies></dependencies>
```

### 使用 Idea 创建

1. 选择 maven-archetype-webapp
2. 修改默认 2.3 版本改用本地 Tomcat 的约束

### 使用 Eclipse 创建

1. 右键工程 -> Properties -> Project Facets -> 去掉 Dynamic Web Module 的勾然后 apply -> 修改需要的版本再勾上 -> 填写需要生成的 webapp 文件位置 src/main/webapp
