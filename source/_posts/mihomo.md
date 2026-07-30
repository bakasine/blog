---
title: Mihomo 裸核运行
date: 2026-01-02 22:01:28
tags:
    - mihomo
    - nssm
    - port
categories: 
    - windows
    - mac
    - tool
---

[Windows 步骤](#win)
  + [安装 nssm 并配置服务](#nssm)
  + [mihomo 关键配置](#config)
[Mac 步骤](#mac)
  + [安装 mihomo 服务](#install)

# <h1 id="win">Windows 步骤</h1>

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
external-ui-url: "https://gh-proxy.org/https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
```

# <h1 id="mac">Mac 步骤</h1>

# <h2 id="instll">安装 mihomo 服务并配置</h2>

`安装`

```
# homebrew
brew install mihomo

# macports
sudo port install mihomo
```

`配置`

```
# 把 iCloud 的配置文件链接到指定目录 (可选)
ln -s ~/Library/Mobile\ Documents/iCloud\~com\~metacubex\~ClashX/Documents /opt/local/etc/mihomo/config.yaml
```

`Macports`

```
## 启动服务 + 开机启动
sudo port load mihomo

## 重启服务
sudo port reload mihomo

## 关闭服务
sudo port unload mihomo
```

`Homebrew`

```
## 启动服务 + 开机启动
sudo brew services start mihomo

## 重启服务
sudo brew services restart mihomo

## 关闭服务
sudo brew services stop mihomo
```