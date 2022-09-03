---
title: xray架设trojan节点
date: 2022-09-03 11:27:45
tags:
	- trojan
---


[安装xray](#install)
[安装nginx](#nginx)
[申请证书](#tls)
[一键安装脚本](#script)


# <h2 id="prepare">准备工作</h2>

下载最新的[xray包](https://p4gefau1t.github.io/trojan-go/basic/full-config/)

``` bash

wget https://github.com/XTLS/Xray-core/releases/download/v1.5.10/Xray-linux-64.zip

# 解压到root目录下的xray文件夹 
unzip Xray-linux-64.zip -d /root/xray/
cd /root/xray

# 下载配置文件模板, 根据注释修改
wget https://raw.githubusercontent.com/XTLS/Xray-examples/main/Trojan-TCP-XTLS/config_server.json -O /root/xray/config.json

```

# <h2 id="nginx">安装nginx申请证书</h2>

``` bash
# 安装nginx
apt update & apt install -y nginx

# 安装证书
wget -O -  https://get.acme.sh | sh
. .bashrc
acme.sh --upgrade --auto-upgrade

acme.sh --issue --server letsencrypt --test -d bak.0012100.xyz -w /home/vpsadmin/www/webpage --keylength ec-256

acme.sh --installcert -d bak.0012100.xyz  --ecc --force


// https://html5up.net/ 随便找一个
wget -O web.zip --no-check-certificate {}
unzip web.zip


// 把 server里的内容修改
// server{
//    listen 80;
//    server_name 你的域名;
//    index index.html;
//    root /home/wwwroot/;
//}
vim /etc/nginx/sites-enabled/default

```


# <h2 id="tls"></h2>

``` bash
wget -O -  https://get.acme.sh | sh
. .bashrc
acme.sh --upgrade --auto-upgrade

acme.sh --issue --server letsencrypt --test -d bak.0012100.xyz -w /home/vpsadmin/www/webpage --keylength ec-256

acme.sh --installcert -d 你的域名 --fullchainpath /data/你的域名/fullchain.crt --keypath /data/你的域名/privkey.key --ecc --force

```




# <h2 id="script">一键安装脚本</h2>

```bash
bash <(curl -sL https://raw.githubusercontent.com/phlinhng/v2ray-tcp-tls-web/master/install.sh) && v2script
```
