---
title: xray架设trojan节点
date: 2022-09-03 11:27:45
tags:
	- trojan
	- xray
categories: 
    - tools
---

> 2022-11-13 17:22:21 更新一键安装脚本

[一键安装](#script)
[安装Nginx](#nginx)
[申请证书](#tls)
[安装Xray](#install)
[给Xray配置TLS证书](#usetls)
[配置Xray](#configurate)
[优化](#optimization)

# <h2 id="script">一键安装</h2>

```bash
wget -N --no-check-certificate -q -O xray.sh "https://raw.githubusercontent.com/uerax/xray-script/master/xray.sh" && chmod +x xray.sh && bash xray.sh
```

# <h2 id="nginx">安装nginx</h2>

* __不推荐centos, 太折腾了__

``` bash
# ubuntu debian
sudo apt update && sudo apt install -y nginx 
mkdir -p /home/xray/webpage/ && cd /home/xray/webpage/
# https://html5up.net/ 随便找一个
apt install unzip && wget -O web.zip --no-check-certificate https://html5up.net/phantom/download && unzip web.zip && rm web.zip
```
 
<h3>修改nginx.conf</h3>

``` bash

# 去除80端口默认占用
sed -i '/\/etc\/nginx\/sites-enabled\//d' /etc/nginx/nginx.conf

# 复制全部 start
cat>/etc/nginx/conf.d/xray.conf<<EOF
server {
	listen 80;
	server_name yourdomain;
	root /home/xray/webpage/;
	index index.html;
}
EOF
# 复制全部 end

# 你的域名 替换
sed -i 's/yourdomain/你的域名/' /etc/nginx/conf.d/xray.conf

systemctl reload nginx

# 访问 http://你的域名 显示正常则成功
```

# <h2 id="tls">申请证书</h2>

``` bash
wget -O -  https://get.acme.sh | sh && cd ~ && . .bashrc
acme.sh --upgrade --auto-upgrade
acme.sh --issue --server letsencrypt --test -d 你的域名 -w /home/xray/webpage --keylength ec-256
# 显示证书和4行cert才成功

acme.sh --set-default-ca --server letsencrypt
acme.sh --issue -d 你的域名 -w /home/xray/webpage --keylength ec-256 --force

```

# <h2 id="install">安装Xray</h2>

# <h3>脚本安装</h3>
``` bash
wget https://github.com/XTLS/Xray-install/raw/main/install-release.sh && bash install-release.sh && rm install-release.sh

```

# <h3>手动安装</h3>

# [xray包](https://p4gefau1t.github.io/trojan-go/basic/full-config/)

``` bash
# 解压到root目录下的xray文件夹 
wget https://github.com/XTLS/Xray-core/releases/download/v1.5.10/Xray-linux-64.zip -O xray.zip && unzip xray.zip -d /root/xray/ && rm xray.zip

# 创建 systemd 部署 start
cat>/etc/systemd/system/xray.service<<EOF
[Unit]
Description=Xray Service
Documentation=https://github.com/xtls
After=network.target nss-lookup.target
[Service]
User=root
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true
ExecStart=/root/xray/xray run -config /usr/local/etc/xray/config.json
Restart=on-failure
RestartPreventExitStatus=23
LimitNPROC=10000
LimitNOFILE=1000000
[Install]
WantedBy=multi-user.target
EOF
# end
```

# <h2 id="usetls">给Xray配置TLS证书</h2>

``` bash
mkdir -p /home/xray/xray_cert && acme.sh --install-cert -d 你的域名 --ecc --fullchain-file /home/xray/xray_cert/xray.crt --key-file /home/xray/xray_cert/xray.key && chmod +r /home/xray/xray_cert/xray.key
```

# <h3>自动更新临期证书</h3>

``` bash
# 创建并写入
cat>/home/xray/xray_cert/xray-cert-renew.sh<<EOF
#!/bin/bash

/root/.acme.sh/acme.sh --install-cert -d yourdomain --ecc --fullchain-file /home/xray/xray_cert/xray.crt --key-file /home/xray/xray_cert/xray.key
echo "Xray Certificates Renewed"

chmod +r /home/xray/xray_cert/xray.key
echo "Read Permission Granted for Private Key"

sudo systemctl restart xray
echo "Xray Restarted"
EOF

# 你的域名 替换
sed -i 's/yourdomain/你的域名/'  /home/xray/xray_cert/xray-cert-renew.sh
```

<h3>创建定时任务</h3>

``` bash
chmod +x /home/xray/xray_cert/xray-cert-renew.sh

( crontab -l | grep -v "0 1 1 * *   bash /home/xray/xray_cert/xray-cert-renew.sh"; echo "0 1 1 * *   bash /home/xray/xray_cert/xray-cert-renew.sh" ) | crontab -
```

# <h2 id="configurate">配置Xray</h2>

``` bash
xray uuid

# 自定义日志 可选 start
# 默认日志位置 /var/log/xray
mkdir /home/xray/xray_log && touch /home/xray/xray_log/access.log && touch /home/xray/xray_log/error.log && chmod a+w /home/xray/xray_log/*.log
# end
```

# <h3>模板文件修改</h3>

# [配置文件模板库](https://github.com/XTLS/Xray-examples)

``` bash
wget https://raw.githubusercontent.com/XTLS/Xray-examples/main/Trojan-TCP-XTLS/config_server.json -O /usr/local/etc/xray/config.json

sed -i 's/\/path\/to\/cert/\/home\/xray\/xray_cert\/xray.crt/' /usr/local/etc/xray/config.json

sed -i 's/\/path\/to\/key/\/home\/xray\/xray_cert\/xray.key/' /usr/local/etc/xray/config.json

```
# <h3>启动Xray</h3>

``` bash
// 脚本安装方式
systemctl start xray && systemctl enable xray
// 手动安装方式

```

# <h2 id="optimization">优化</h2>

# <h3>开启bbr</h3>

# {% post_link bbr 开启bbr加速 %}

# <h3>开启 HTTP 自动跳转 HTTPS</h3>

``` bash

sed -i '/\/home\/xray\/webpage\//d' /etc/nginx/conf.d/xray.conf
sed -i '/index/d' /etc/nginx/conf.d/xray.conf

# 在80端口规则最后加入 可同时删除root和index两行
sed -i '3a \\treturn 301 https://$http_host$request_uri;' /etc/nginx/conf.d/xray.conf

#在加入新的server
cat>>/etc/nginx/conf.d/xray.conf<<EOF
server {
   listen 127.0.0.1:8080;
   root /home/xray/webpage/;
   index index.html;
   add_header Strict-Transport-Security "max-age=63072000" always;
}
EOF
#end

systemctl restart nginx

#修改xray的fallback端口为8080 "dest": 80 -> 改成 "dest": 8080
sed -i '19,24d' /usr/local/etc/xray/config.json

sudo sed -i 's/\"dest\".*/"dest": 8080/g' /usr/local/etc/xray/config.json


systemctl restart xray
```