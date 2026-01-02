---
title: 自建 Vaultwarden 密码管理
date: 2026-01-02 21:51:28
tags:
    - vaultwarden
categories: 
    - linux
    - tool
---

[安装](#ins)
[配置 Caddy](#caddy)
[创建用户](#register)
[安装插件](#plugins)

# <h2 id="ins">安装</h2>

`docker安装`

```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "域名"
      SIGNUPS_ALLOWED: "false"
      ADMIN_TOKEN: '管理密码'
      ADMIN_RATELIMIT_SECONDS: 300
      ADMIN_RATELIMIT_MAX_BURST: 5
    volumes:
      - ./vw-data/:/data/

  caddy:
    image: caddy:latest
    container_name: caddy-1
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./conf:/etc/caddy
      - ./site:/srv
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - vaultwarden

volumes:
  caddy_data:
  caddy_config:
```

# <h2 id="caddy">配置 Caddy</h2>

`在 conf 文件夹下创建 Caddyfile 文件`

```
域名 {
    reverse_proxy vaultwarden:80
}
```

# <h2 id="register">创建用户</h2>

```
由于关闭了注册, 需要管理员邀请才能注册

访问 https://域名/admin/ 然后输入管理密码

然后进入 Users 界面, 输入要注册的邮箱

然后访问注册页面注册对应的邮箱用户
```

# <h2 id="plugins">安装插件</h2>

[插件地址](https://chromewebstore.google.com/detail/bitwarden-password-manage/nngceckbapebfimnlniiiahkandclblb)

```
登录用户后直接根据流程就即可
```