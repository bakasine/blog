---
title: oracle免费云服务
date: 2022-09-03 09:46:35
tags:
	- vps
categories: 
    - vps
---

[注册](#register)
[创建实例](#instance)
[重装系统](#reos)
[后记](#postscript)

# <h2 id="register">注册</h2>

# [oracle](https://www.oracle.com/cloud/sign-in.html)


# <h2 id="instance">创建实例</h2>

`Launch resources` --> `Create a VM instance` --> `Image and shape` --> `Add SSH keys` --> `Boot volume` --> `Specify a...` 

# <h2 id="reos">重装系统</h2>

``` bash
# debian 10  (-firmware 额外驱动支持, 默认密码MoeClub.org)
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -p 'password' -d 10.3 -v 64 -a -firmware

# ubuntu 20.04
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -p 'password' -u 20.04 -v 64 -a -firmware
```

