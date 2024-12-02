---
title: 通过 ZeroTier 将多台设备组成内网
date: 2024-12-03 19:19:39
tags:
	- zerotier
categories: 
    - network
---

[环境准备](#prepare)
[配置服务端](#server)
[配置客户端](#client)
[用法](#usage)

# <h2 id="prepare">环境准备</h2>

```bash
# Git
apt install -y git

# Docker
curl -fsSL https://get.docker.com |bash
```

# <h2 id="server">配置服务端</h2>

1. 安装并部署 ZeroTier 服务端

```bash
# 安装 ZeroTier
git clone https://github.com/xubiaolin/docker-zerotier-planet.git
cd docker-zerotier-planet
./deploy.sh
# 安装完成后通过 3443 端口访问 ZeroTier

# docker-zerotier-planet/data/zerotier/dist/planet 
# 将 planet 文件发送到客户端的机器上
```

2. 访问 http://ip:3443 进入管理页面, 默认账号：`admin` 密码：`password`
3. 使用 登录后，点击选项卡中的 `Add Network` 来创建一个网络。
4. 点击上方 `IPv4 Assign Mode` ，再勾选 `Auto-assign from IP Assignment Pool`
5. 复制下括号里的网络ID 

# <h2 id="client">配置客户端</h2>

1. [ZeroTier 下载](https://www.zerotier.com/download/)
2. 将 planet 文件复制到:
linux: `/var/lib/zerotier-one/`
windows: `C:\ProgramData\ZeroTier\One` 
3. 重新启动 ZeroTier `service zerotier-one restart`
4. 加入服务端的网络 `zerotier-cli join 服务端的网络ID`
5. 前往服务端的控制页面，勾选 `Authorized` 