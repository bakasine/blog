---
title: Mihomo 裸核运行
date: 2026-01-02 22:01:28
tags:
    - mihomo
    - nssm
categories: 
    - windows
    - tool
---

[安装 nssm 并配置服务](#nssm)
[mihomo 关键配置](#config)

# <h2 id="nssm">安装 nssm 并配置服务</h2>

```
nssm.exe install mihomo
Application里
path选择mihomo.exe完整路径, dir选择所在的目录, arguments填写-f config.yaml完整路径
install service
```

# <h2 id="config">mihomo 关键配置</h2>

```
external-ui: ./ui
external-ui-name: xd
# 目前支持下载zip,tgz格式的压缩包
external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
```

