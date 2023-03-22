---
title: SSR一键安装整合
date: 2020-02-23 23:19:36
cover: /images/preview8.jpg
coverWidth: 1600
coverHeight: 900
tags: 
        - SSR
        - debian
        - ubuntu
categories: 
        - linux
---

> 逗比

``` bash

wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh

备份:  wget -N --no-check-certificate https://www.vrrmr.net/55R/SSR.sh && chmod +x SSR.sh && bash SSR.sh
ShadowsocksR 安装后，自动设置为 系统服务，所以支持使用服务来启动/停止等操作，同时支持开机启动。

启动 ShadowsocksR：/etc/init.d/ssr start
停止 ShadowsocksR：/etc/init.d/ssr stop
重启 ShadowsocksR：/etc/init.d/ssr restart
查看 ShadowsocksR状态：/etc/init.d/ssr status
ShadowsocksR 默认支持UDP转发，服务端无需任何设置。

```

> 91大神

``` bash

 wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/shadowsocks_install/master/ssr-install.sh && bash ssr-install.sh
备用代码：

wget -N --no-check-certificate https://www.vrrmr.net/55R/ssr-install.sh && bash ssr-install.sh
管理命令：

添加用户：ssr adduser
删除用户：ssr deluser
启动SSR：ssr start
停止SSR：ssr stop
重启SSR：ssr restart
卸载SSR：ssr uninstall
更新SSR：ssr update
修改用户和的加密，混淆和协议的话：

修改vi /home/ssr/mudb.json文件

```

> 秋水大神

``` bash

一键安装 Shadowsocks-Python， ShadowsocksR， Shadowsocks-Go， Shadowsocks-libev 版（四选一）服务端.

wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
备用：

wget --no-check-certificate -O shadowsocks-all.sh https://www.vrrmr.net/55R/shadowsocks-all.sh
卸载代码：

./shadowsocks-all.sh uninstall
启动脚本后面的参数含义，从左至右依次为：启动，停止，重启，查看状态。

Shadowsocks-Python 版：
/etc/init.d/shadowsocks-python start | stop | restart | status

ShadowsocksR 版：
/etc/init.d/shadowsocks-r start | stop | restart | status

Shadowsocks-Go 版：
/etc/init.d/shadowsocks-go start | stop | restart | status

Shadowsocks-libev 版：
/etc/init.d/shadowsocks-libev start | stop | restart | status
各版本默认配置文件

Shadowsocks-Python 版：
/etc/shadowsocks-python/config.json

ShadowsocksR 版：
/etc/shadowsocks-r/config.json

Shadowsocks-Go 版：
/etc/shadowsocks-go/config.json

Shadowsocks-libev 版：
/etc/shadowsocks-libev/config.json

```