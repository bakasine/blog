---
title: Debian(Ubuntu)网络安装/重装系统一键脚本
date: 2018-06-26 23:19:36
tags: 
        - linux
        - debian
        - ubuntu
categories: 
        - linux
---

> 更新于 2023-11-26 22:12:21

```
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/MoeClub/Note/master/InstallNET.sh') -d 11 -v 64 -p "password" -port "2222"
```

`国内机器`

```
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/MoeClub/Note/master/InstallNET.sh') -d 11 -v 64 -p "password" -port "2222" --mirror 'https://mirrors.cloud.tencent.com/debian/'
```

---

> 注意

1. 全自动安装默认root密码: MoeClub.org,安装完成后请立即更改密码
2. 请使用 passwd root 命令更改密码
3. OpenVZ构架不适用

> 确保安装了所需软件

``` bash
#Debian/Ubuntu:
apt-get install -y gawk sed grep
 
#RedHat/CentOS:
yum install -y gawk sed grep
```

> 如果出现了错误,请运行

``` bash
#Debian/Ubuntu:
apt-get update

#RedHat/CentOS:
yum update
```

> 自用debian9
``` bash
wget --no-check-certificate -qO DebianNET.sh 'https://raw.githubusercontent.com/bakasine/Scripts/main/DebianNET.sh' && chmod a+x DebianNET.sh

bash DebianNET.sh -d 11 -v 64 -p 密码 -a
```

> 一键下载

``` bash
wget --no-check-certificate -qO DebianNET.sh 'https://raw.githubusercontent.com/bakasine/Scripts/main/DebianNET.sh' && chmod a+x DebianNET.sh
```

> 全自动/非自动示例

* 全自动安装

``` bash
bash DebianNET.sh -d wheezy -v i386 -a
```

* VNC手动安装

``` bash
bash DebianNET.sh -d wheezy -v i386 -m
```

* 全自动安装(指定网络参数)

``` bash
# 将X.X.X.X替换为自己的网络参数.
# --ip-addr :IP Address/IP地址
# --ip-gate :Gateway   /网关
# --ip-mask :Netmask   /子网掩码
bash DebianNET.sh -d wheezy -v i386 -a --ip-addr X.X.X.X --ip-mask X.X.X.X --ip-gate X.X.X.X
```

> 使用示例

* 【默认】安装Debian 7 x32

``` bash
bash DebianNET.sh -d wheezy -v i386
```

``` bash
bash DebianNET.sh -d 7 -v 32
```

* 安装Debian 8 x64

``` bash
bash DebianNET.sh -d jessie -v amd64
```

``` bash
bash DebianNET.sh -d 8 -v 64
```

* 安装Debian 9 x64

``` bash
bash DebianNET.sh -d stretch -v amd64
```

``` bash
bash DebianNET.sh -d 9 -v 64
```

* 安装Ubuntu 14.04 x64

``` bash
bash DebianNET.sh -u trusty -v 64
```

* 安装Ubuntu 16.04 x64

``` bash
bash DebianNET.sh -u xenial -v 64
```

* 安装Ubuntu 18.04 x64

``` bash
bash DebianNET.sh -u bionic -v 64
```

> 转载自

# [萌咖](https://github.com/MoeClub/Note)