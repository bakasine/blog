---
title: V2Ray+WebSocket+TLS+Nginx一键安装脚本
date: 2020-02-23 23:19:36
cover: /images/preview9.jpg
coverWidth: 1600
coverHeight: 900
tags: 
        - linux
        - debian
        - ubuntu
categories: 
        - proxy
---

> 更新于 2022-09-14 22:12:21

__脚本过老已不推荐使用,而且不安全__
__建议使用__ __{% post_link trojan-build trojan搭建 %}__ __的方式搭建__

---

1. 准备一个域名解析到当前服务器ip

> 一键安装脚本（二选一）

* 脚本适用于：Debian 9+ / Ubuntu 18.04+ / Centos7+

1.Vmess+websocket+TLS+Nginx+Website（推荐）

``` bash
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh) | tee v2ray_ins.log
```

2.Vmess + HTTP2 over TLS

``` bash
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install_h2.sh) | tee v2ray_ins_h2.log
``` 

> 脚本管理

``` bash
#启动 V2ray：
systemctl start v2ray

#停止 V2ray：
systemctl stop v2ray

#启动 Nginx：
systemctl start nginx

#停止 Nginx：
systemctl stop nginx

#Web 目录：
/home/wwwroot/levis

#V2ray 服务端配置：
/etc/v2ray/config.json

#V2ray 客户端配置：
执行安装时所在目录下的 v2ray_info.txt

#Nginx 目录：
/etc/nginx

#证书目录：
/data/v2ray.key 和 /data/v2ray.crt

```

> v2rayN Windows客户端下载/配置

``` bash
Windows客户端下载地址：http://down.wangchao.info/soft/v2rayN.zip
```

> 转载自

# [逗逼](https://doubibackup.com/v2ray-ws-tls-nginx-script.html)
