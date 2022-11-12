---
title: bbr加速
date: 2022-09-27 14:16:24
tags: 
    - bbr
    - linux
categories: 
    - linux
---

# <h2 id="openbbr">开启BBR</h2>

linux内核版本大于4.9的系统自带的bbr

* Debian 9+
* Ubuntu 17.04+
* CentOS 8+

``` bash
# debian10+ 可用
echo 'deb http://deb.debian.org/debian buster-backports main' >> /etc/apt/sources.list

apt update && apt -t buster-backports install linux-image-amd64

# Ubuntu 跳过前两步
echo net.core.default_qdisc=fq >> /etc/sysctl.conf
echo net.ipv4.tcp_congestion_control=bbr >> /etc/sysctl.conf
sysctl -p
reboot
```

# <h3>确认</h3>
输入 `lsmod | grep bbr` 返回 `tcp_bbr`
输入 `lsmod | grep fq` 返回 `sch_fq`
输入 `sysctl net.ipv4.tcp_available_congestion_control` 返回 `net.ipv4.tcp_available_congestion_control = bbr cubic reno` 