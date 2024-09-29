---
title: 通过 Frp 实现内网穿透
date: 2024-09-23 19:19:39
tags:
	- frp
categories: 
    - tool
---

[安装](#ins)
[配置服务端](#server)
[配置客户端](#client)
[用法](#usage)

# <h2 id="ins">安装</h2>

[下载链接](https://github.com/fatedier/frp/releases/latest)

# <h2 id="server">配置服务端</h2>

```bash
mkdir -p /root/tmp
tar --strip-components=1 -C /root/tmp -xvf 安装包
mv -f /root/tmp/frps /usr/local/bin/azi

cat > /etc/systemd/system/azi.service <<EOF
[Unit]
Description = azi server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
ExecStart = /usr/local/bin/azi -c /usr/local/etc/azi/config.toml
Restart=on-failure
RestartSec=5

[Install]
WantedBy = multi-user.target
EOF

systemctl enable azi

mkdir -p /usr/local/etc/azi/

cat > /usr/local/etc/azi/config.toml <<EOF
bindPort = 8848
bindAddr = "::"
auth.token = "hentailolicon"
EOF

rm -r /root/tmp
```

# <h2 id="client">配置客户端</h2>

```toml
serverAddr = "服务端的IP"
serverPort = 8848
auth.token = "hentailolicon"

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "::"
localPort = 22
remotePort = 2222
transport.useEncryption = true
transport.useCompression = true
```

`Win11 安装 OpenSSH`

```
设置 -> 系统 -> 可选功能 -> 添加可选功能 -> 搜索 OpenSSH

搜索 OpenSSH 客户端 OpenSSH 服务器 -> 下一步 -> 安装

win+r -> services.msc -> OpenSSH SSH Server -> 启动
```

`开放端口`

```
win+r -> firewall.cpl -> 高级设置 -> 入站规则 -> 新建规则
```

# <h2 id="usage">用法</h2>

```bash
ssh -p 2222 用户名@服务端ip
```