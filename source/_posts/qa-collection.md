---
title: 开发遇见的问题合集
date: 2023-10-26 02:44:26
tags:
    - qa
categories: 
    - qa
---

[__SSH相关问题__](#ssh)
[SSH如何保持连接不自动断开](#ssh_keepalive)
[__Mac M1 遇到的问题__](#mac)

# <h2 id="ssh">SSH相关问题</h2>

# <h3 id="ssh_keepalive">SSH如何保持连接不自动断开</h2>

```bash
cat > ~/.ssh/config << EOF
Host *
    ServerAliveInterval 60
EOF
```

# <h2 id="mac">Mac M1 遇到的问题</h2>

# {% post_link mac-issue Mac M1 遇到的问题 %}

