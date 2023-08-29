---
title: Clash 基础用法
date: 2023-08-29 14:43:59
tags: 
    - clash
    - cfw
categories: 
    - note
---

# <h2 id="ins">安装</h2>

`Meta内核`

[__Clash.Meta__](https://github.com/MetaCubeX/Clash.Meta/releases)

`客户端`

[__ClashX.Meta__](https://github.com/MetaCubeX/ClashX.Meta/releases)
[__Clash for Windows__](https://github.com/Fndroid/clash_for_windows_pkg/releases)
[__Clash Verge__](https://github.com/zzzgydi/clash-verge/releases)

# <h2 id="core">修改内核</h2>

__Mac上可以直接使用ClashX.Meta原生支持3种Clash Core, 如果使用Clash for Windows默认不支持Meta Core, 但是可以手动更换__

用Meta.Clash的内核更换

`resources` -> `static` -> `files` -> `win` -> `x64` -> `clash-win64.exe`

# <h2 id="tun">如何开启tun模式</h2>

`Clash for Windows`

__Clash for Windows要启动tun模式需要安装Service Mode, 但是由于他做了内核校验我们更换成Meta内核后会安装失败__
__所以可以考虑使用管理员打开应用强行启动tun模式__

`Clash Verge`

__Verge有便携版和安装包,根据issue反馈发现便携版开启tun会有一些问题,而使用安装包则不会出现问题__