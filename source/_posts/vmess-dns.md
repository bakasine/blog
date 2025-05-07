---
title: vmess/vless + ws + tls + dns
date: 2022-10-15 10:52:47
tags:
    - vmess
    - vless
    - xray
categories: 
    - tools
---

[准备](#prepare)
[Vmess](#vmess)
[Vless](#vless)

# <h2 id="prepare">准备</h2>

# {% post_link trojan-build 安装nginx和申请证书 %}

# <h2 id="vmess">Vmess</h2>

__xray config__

```json
{
  "log":{
    "access": "/var/log/xray/access.log",
    "error": "/var/log/xray/error.log",
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "port": 1919,
      "listen": "127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [{
          "id": "",
          "alterID": 0
        }]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/crayfish"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "protocol": ["bittorrent"],
        "outboundTag": "blocked"
      }
    ]
  }
}
```

__nginx config__

```json
server {
        listen 443 ssl;
        server_name 你的域名;

        index index.html;
        root /home/xray/webpage/;

        ssl_certificate /home/xray/xray_cert/xray.crt;
        ssl_certificate_key /home/xray/xray_cert/xray.key;
        ssl_protocols         TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers           HIGH:!aNULL:!MD5;

        # 在 location 
        location /crayfish {
        proxy_pass http://127.0.0.1:1919;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

# <h2 id="vless">Vless</h2>

```json
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "listen": "/dev/shm/Xray-VLESS-WSS-Nginx.socket,0666",
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "" // 填写你的 UUID
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/crayfish" // 填写你的 path
        }
      }
    }
  ],
  "outbounds": [
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "blocked",
      "protocol": "blackhole",
      "settings": {}
    }
  ],
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked"
      }
    ]
  }
}
```

__nginx config__

```json
server {
	listen 443 ssl http2;
	server_name 你的域名;

	index index.html;
	root /home/xray/webpage;

	ssl_certificate /home/xray/xray_cert/xray.crt
	ssl_certificate_key /home/xray/xray_cert/xray.key;
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
	
	# 在 location 后填写 /你的 path
	location /crayfish {
	if ($http_upgrade != "websocket") {
		return 404;
	}
        proxy_pass http://unix:/dev/shm/Xray-VLESS-WSS-Nginx.socket;
	proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_read_timeout 52w;
    }
}
```