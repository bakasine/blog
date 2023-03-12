---
title: 通过 Cloudflare 自建免费图床
date: 2023-03-11 22:17:05
tags:
    - image
categories: 
    - note
---

[__一.准备__](#pre)
[__二.搭建__](#build)
[__三.域名绑定__](#domain)
[__四.后台管理__](#manage)
[__五.其他__](#other)

# <h2 id="pre">一.准备</h2>

__1.注册 [Cloudflare](https://www.cloudflare.com/zh-cn/)__
__2.注册 [Github](https://github.com/) (可选)__
__3.购买域名(可选)__

# <h2 id="build">二.搭建</h2>

__`登录cloudflare` -> 点击左边列表的`page` -> 点击`创建项目`__

![1](img-hosting-1.png)

__1.如果没有github,则下载 [Telegraph-Image](https://github.com/cf-pages/Telegraph-Image/archive/refs/heads/main.zip) 点击`直接上传`, 跟着填写部署即可__

__2.有github,则fork项目 [Telegraph-Image](https://github.com/cf-pages/Telegraph-Image) , 然后点击`连接到Git`, 点击`添加账户`后登录你的Github同意绑定,然后选择一个存储库选择`Telegraph-Image`,最后点`开始设置`即可__

# <h2 id="domain">三.域名绑定</h2>

__回到用户首页 -> `page` -> `Telegraph-Image` -> `自定义域` -> `设置自定义域`__

![2](img-hosting-2.png)

# <h2 id="manage">四.后台管理</h2>

__`Workers` -> `KV` -> `创建命名空间` -> `添加`__

![3](img-hosting-3.png)

__`page` -> `Telegraph-Image` -> `设置` -> `函数` -> `KV 命名空间绑定` -> `编辑绑定` -> 变量名称`img_url` -> KV命名空间选择 -> `保存`__

![4](img-hosting-4.png)

__`page` -> `Telegraph-Image` -> `设置` -> `环境变量` -> `制作` -> `编辑变量` -> 账号`BASIC_USER` -> 密码`BASIC_PASS` -> `保存`__

![5](img-hosting-5.png)

# <h2 id="other">五.其他</h2>

__其他功能教程前往 [Telegraph-Image](https://github.com/cf-pages/Telegraph-Image/blob/main/README.md)__
