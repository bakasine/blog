---
title: MacBook(M1)遇到的问题
date: 2021-10-09 00:26:28
tags:
    - mac
    - issue
categories:
    - mac
---

* [MySQL问题](#mysql)
* [Docker问题](#docker)

### <h2 id="mysql">MySQL</h2>

* 通过docker安装mysql

``` bash
// mysql 暂不支持 arm架构，所以选用 mysql-server
docker pull mysql/mysql-server

docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql/mysql-server

// 因为只有8.0的版本支持arm架构，所以需要修改配置后才能通过 navicat 连接
docker exec -it mysql mysql -uroot -p

use mysql;

update user set host='%' where user='root';

flush privileges;
```

### <h2 id="docker">Docker</h2>

* 通过docker安装nginx

``` bash
// m1的docker容器与windows不同，类似于虚拟机
// 容器访问宿主机不能直接使用localhost要使用
docker.for.mac.host.internal

```



