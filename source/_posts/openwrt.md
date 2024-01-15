---
title: N1 盒子 Openwrt 系统刷入
date: 2024-01-16 14:43:59
tags: 
    - openwrt
    - openclash
categories: 
    - note
---

[准备](#pre)
[降级(可选)](#down)
[刷机](#flash)
[旁路由](#side)

# <h2 id="pre">准备</h2>

下载固件[Flippy版](https://t.me/openwrt_flippy/4726),[官方版本](https://openwrt.ai/?target=armsr%2Farmv8&id=box)
下载[balenaEtcher](https://github.com/balena-io/etcher/releases)
下载[降级脚本](https://github.com/sorrymyself/P1-N1)(版本>2.19需要)
下载[adb命令工具](https://github.com/awake558/adb-win)

# <h2 id="down">降级</h2>

__N1 盒子接上电源和显示器, 版本号大于2.19就需要进行降级__
__插上网线或者点击右下角连接 WIFI 后会显示 N1 盒子的 IP__
__插上无线鼠标点击4下版本号, 会显示adb打开__
__同一个网络下的电脑运行降级脚本 run.bat 输入 N1 盒子的 IP__

# <h2 id="flash">刷机</h2>

1. 电脑插上 U盘,打开 `balenaEtcher` 选择固件解压的 `img文件` 点击 Flash
2. 拔下 N1 盒子的电源, 插上 U 盘后再接入电源
3. 打开 adb.exe, 输入 `adb connect 盒子的IP`, 然后输入 `adb reboot update`
4. 连接 N1 盒子发出的 Wi-Fi, 电脑访问 192.168.1.1 进入管理页面, 默认密码：password
5. 点击左边的 `系统` -> `TTYD终端`, 用户名: `root`, 密码: `password`
6. 执行命令 `cd /root && ./install-to-emmc.sh`
7. 根据不同的机器输入对应的数字, N1 盒子为 `11`, 执行完最后输入 `1`
8. 显示 `Successful···` 即安装成功, 拔掉 U盘 和 N1 电源线，重新插入启动即可

# <h2 id="side">旁路由</h2>

`网络` -> `接口` -> `LAN`

`一般配置`
协议: 静态协议
IPv4 地址: 主路由 IP 如果是 192.168.1.1, 就把最后位的 1 改成 255 以内的数
IPv4 网关: 填主路由的 IP, 一般为 192.168.1.1, 可以用 ipconfig 命令查看
使用自定义的 DNS 服务器: 和IPv4 网关一致

`DHCP 服务器`

忽略此接口: 勾选
IPv6 设置: 前三项已禁用

* 接下来解决 IPv6 失效问题

再点击`接口` -> `添加新接口`

接口的名称: lan6
新接口协议: DHCPv6 客户端
自定义接口: @lan

创建后, 防火墙设置 `lan`

点击右下角的 `保存&应用`

`网络` -> `防火墙` -> `自定义规则`

__添加后点击重启防火墙__

```
iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
```

`通信规则` -> `打开路由器端口`

名称: all
协议: tcp+udp
外部接口: 不填为全部放行, 或者根据自己需求填写接口

点击添加后, 点击保存&应用

