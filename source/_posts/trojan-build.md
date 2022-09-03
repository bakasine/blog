---
title: xray架设trojan节点
date: 2022-09-03 11:27:45
tags:
	- trojan
---


[安装Nginx](#nginx)
[申请证书](#tls)
[安装Xray](#install)
[给Xray配置TLS证书](#usetls)
[配置Xray](#configurate)
[优化](#optimization)

# <h2 id="nginx">安装nginx</h2>

``` bash
sudo apt update && sudo apt install -y nginx
mkdir -p /home/xray/webpage/ && cd /home/xray/webpage/
# https://html5up.net/ 随便找一个
wget -O web.zip --no-check-certificate https://html5up.net/phantom/download && unzip web.zip && rm web.zip
```

修改nginx.conf

``` bash
vim /etc/nginx/nginx.conf

server {
		listen 80;
		server_name 你的域名;
		root /home/xray/webpage/;
		index index.html;
}

systemctl reload nginx

# 访问 http://你的域名 显示正常则成功
```

# <h2 id="tls">申请证书</h2>

``` bash
wget -O -  https://get.acme.sh | sh
. .bashrc
acme.sh --upgrade --auto-upgrade
acme.sh --issue --server letsencrypt --test -d 你的域名 -w /home/xray/webpage --keylength ec-256
# 显示证书和4行cert才成功

acme.sh --set-default-ca --server letsencrypt
acme.sh --issue -d 你的域名 -w /home/xray/webpage --keylength ec-256 --force

```

# <h2 id="install">安装Xray</h2>

# <h3>脚本安装</h3>
``` bash
wget https://github.com/XTLS/Xray-install/raw/main/install-release.sh

bash install-release.sh
rm install-release.sh

```

# <h3>手动安装</h3>

[xray包](https://p4gefau1t.github.io/trojan-go/basic/full-config/)

``` bash
wget https://github.com/XTLS/Xray-core/releases/download/v1.5.10/Xray-linux-64.zip xray.zip

# 解压到root目录下的xray文件夹 
unzip xray.zip -d /root/xray/ && rm xray.zip

# 下载配置文件模板, 根据注释修改
wget https://raw.githubusercontent.com/XTLS/Xray-examples/main/Trojan-TCP-XTLS/config_server.json -O /root/xray/config.json

```

# <h2 id="usetls">给Xray配置TLS证书</h2>

``` bash
mkdir /home/xray/xray_cert
acme.sh --install-cert -d 你的域名 --ecc --fullchain-file /home/xray/xray_cert/xray.crt --key-file /home/xray/xray_cert/xray.key
chmod +r /home/xray/xray_cert/xray.key
```

# <h3>自动更新临期证书</h3>


``` bash
vim /home/xray/xray_cert/xray-cert-renew.sh
```
复制以下内容
``` bash
#!/bin/bash

/root/.acme.sh/acme.sh --install-cert -d 你的域名 --ecc --fullchain-file /home/xray/xray_cert/xray.crt --key-file /home/xray/xray_cert/xray.key
echo "Xray Certificates Renewed"

chmod +r /home/xray/xray_cert/xray.key
echo "Read Permission Granted for Private Key"

sudo systemctl restart xray
echo "Xray Restarted"
```

创建定时任务

``` bash
chmod +x /home/xray/xray_cert/xray-cert-renew.sh
crontab -e

#添加以下内容到最底部
# 1:00am, 1st day each month, run `xray-cert-renew.sh`
0 1 1 * *   bash /home/xray/xray_cert/xray-cert-renew.sh
```

# <h2 id="configurate">配置Xray</h2>

``` bash
xray uuid

# 自定义日志 可选 start
# 默认日志位置 /var/log/xray
mkdir /home/xray/xray_log
touch /home/xray/xray_logaccess.log && touch /home/xray/xray_log/error.log
chmod a+w /home/xray/xray_log/*.log
# end
```

# <h3>手动填写配置</h3>
vim /usr/local/etc/xray/config.json
``` json
// REFERENCE:
// https://github.com/XTLS/Xray-examples
// https://xtls.github.io/config/
// 常用的 config 文件，不论服务器端还是客户端，都有 5 个部分。外加小小白解读：
// ┌─ 1*log 日志设置 - 日志写什么，写哪里（出错时有据可查）
// ├─ 2_dns DNS-设置 - DNS 怎么查（防 DNS 污染、防偷窥、避免国内外站匹配到国外服务器等）
// ├─ 3_routing 分流设置 - 流量怎么分类处理（是否过滤广告、是否国内外分流）
// ├─ 4_inbounds 入站设置 - 什么流量可以流入 Xray
// └─ 5_outbounds 出站设置 - 流出 Xray 的流量往哪里去
{
  // 1\_日志设置
  "log": {
    "loglevel": "warning", // 内容从少到多: "none", "error", "warning", "info", "debug"
    "access": "/home/vpsadmin/xray_log/access.log", // 访问记录
    "error": "/home/vpsadmin/xray_log/error.log" // 错误记录
  },
  // 2_DNS 设置
  "dns": {
    "servers": [
      "https+local://1.1.1.1/dns-query", // 首选 1.1.1.1 的 DoH 查询，牺牲速度但可防止 ISP 偷窥
      "localhost"
    ]
  },
  // 3*分流设置
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      // 3.1 防止服务器本地流转问题：如内网被攻击或滥用、错误的本地回环等
      {
        "type": "field",
        "ip": [
          "geoip:private" // 分流条件：geoip 文件内，名为"private"的规则（本地）
        ],
        "outboundTag": "block" // 分流策略：交给出站"block"处理（黑洞屏蔽）
      },
      // 3.2 屏蔽广告
      {
        "type": "field",
        "domain": [
          "geosite:category-ads-all" // 分流条件：geosite 文件内，名为"category-ads-all"的规则（各种广告域名）
        ],
        "outboundTag": "block" // 分流策略：交给出站"block"处理（黑洞屏蔽）
      }
    ]
  },
  // 4*入站设置
  // 4.1 这里只写了一个最简单的 vless+xtls 的入站，因为这是 Xray 最强大的模式。如有其他需要，请根据模版自行添加。
  "inbounds": [
    {
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "", // 填写你的 UUID
            "flow": "xtls-rprx-direct",
            "level": 0,
            "email": "root@你的域名"
          }
        ],
        "decryption": "none",
        "fallbacks": [
          {
            "dest": 80 // 默认回落到防探测的代理
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "xtls",
        "xtlsSettings": {
          "allowInsecure": false, // 正常使用应确保关闭
          "minVersion": "1.2", // TLS 最低版本设置
          "alpn": ["http/1.1"],
          "certificates": [
            {
              "certificateFile": "/home/xray/xray_cert/xray.crt",
              "keyFile": "/home/xray/xray_cert/xray.key"
            }
          ]
        }
      }
    }
  ],
  // 5*出站设置
  "outbounds": [
    // 5.1 第一个出站是默认规则，freedom 就是对外直连（vps 已经是外网，所以直连）
    {
      "tag": "direct",
      "protocol": "freedom"
    },
    // 5.2 屏蔽规则，blackhole 协议就是把流量导入到黑洞里（屏蔽）
    {
      "tag": "block",
      "protocol": "blackhole"
    }
  ]
}
```

# <h3>模板文件修改</h3>
[配置文件磨板库](#https://github.com/XTLS/Xray-examples)
``` bash
wget https://raw.githubusercontent.com/XTLS/Xray-examples/main/Trojan-TCP-XTLS/config_server.json /usr/local/etc/xray/config.json

```
# <h3>启动Xray</h3>

``` bash
// 脚本安装自动配置, 手动安装需要手动去/etc/systemd/system/填写xray.service文件
systemctl start xray
systemctl enable xray
```

# <h2 id="optimization">优化</h2>

# <h3>开启bbr</h3>
{% post_link bbr 开启bbr加速 %}

# <h3>开启 HTTP 自动跳转 HTTPS</h3>

``` bash
vim /etc/nginx/nginx.conf
# 在80端口规则最后加入 可同时删除root和index两行
return 301 https://$http_host$request_uri;

#在加入新的server
server {
   listen 127.0.0.1:8080;
   root /home/xray/webpage/;
   index index.html;
   add_header Strict-Transport-Security "max-age=63072000" always;
}

#end

systemctl restart nginx

#修改xray的fallback端口为8080 "dest": 80 -> 改成 "dest": 8080
vim /usr/local/etc/xray/config.json
systemctl restart xray
```