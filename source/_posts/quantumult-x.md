---
title: ios去广告、分流、代理
date: 2022-09-01 01:59:46
cover: /images/preview4.jpg
coverWidth: 1600
coverHeight: 900
tags:
	- quantumultx
	- ios
categories: 
    - tool
---

[准备工作](#prepare)
[导入配置](#configuration)
[后记](#postscript)

# <h2 id="prepare">准备工作</h2>

下载相关的工具，目前ios大部分的代理工具都具备此功能，主流的有以下四个。

	Shadowrocket (3刀)
	Quantumult X (8刀)
	Surge 		 (50刀)
	Loon		 (5刀)

我只用过前两个，而去广告需要长时驻留后台，所以选用耗电更少的Quantumult X。目前上述工具都需要非国区账号才可购买。

# <h2 id="configuration">导入配置</h2>

`打开Quantumult X` -> `点击右下角` -> 拉到最下点击`下载配置`
输入 `http://211336.xyz:1919/quantumult.conf` 这是我自己的配置，也可以用网上的
![1](quantumult-x_1.png)

# <h3>开启MitM并信任Quantumult X证书</h3>
`打开Quantumult X` -> `点击右下角` -> `MitM` -> `开启MitM` -> `生成密钥及证书` -> `右上角点保存` -> `允许安装描述文件` -> `关闭` -> `前往手机的设置，不是在Quantumult X` -> `看到已下载描述文件` -> `安装` -> `输入手机的解锁密码` -> `安装` -> `安装` -> `前往手机的设置` -> `通用` -> `关于本机` -> `证书信任设置` -> `找到Quantumult X Custom Root Certificate` -> `点绿它以信任该根证书` -> `继续`

# <h3>开启规则分流</h3>
`打开Quantumult X` -> `长按右下角` -> `选中规则分流` -> `添加自己的节点` -> `漏网之鱼选择你的节点` -> `开启右上角`

# <h2 id="configuration">后记</h2>

如果你不需要代理，只需要去广告。那你可以`删除所有的节点` -> `删除所有的自定义策略`
但是广告过滤列表如果你没有代理会`拉取失败`，所以需要第一次`开启代理`。
只去广告的话时间久了不更新策略会出现广告`过滤失败`，因为广告列表需要经常更新。
所以至少需要一个代理来保证策略的`实时性`才能有完整的体验