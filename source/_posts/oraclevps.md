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
[~~重装系统~~优化系统](#reos)
[后记](#postscript)

# <h2 id="register">注册</h2>

# [oracle](https://www.oracle.com/cloud/sign-in.html)


# <h2 id="instance">创建实例</h2>

`Launch resources` --> `Create a VM instance` --> `Image and shape` --> `Add SSH keys` --> `Boot volume` --> `Specify a...` 
# <h2 id="reos">优化系统</h2>

_dd系统后出现失联的情况, 推荐使用原生系统关闭防火墙使用__

`实例` --> `主要 VNIC` --> `子网` --> `安全列表` --> `添加入站规则` --> `CIDR 0.0.0.0/0 所有协议`

```shell
# ubuntu
# 关闭防火墙
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F

# 卸载防火墙
apt-get purge netfilter-persistent && reboot

# 删除防火墙
rm -rf /etc/iptables && reboot

# centos
# 删除多余附件
systemctl stop oracle-cloud-agent
systemctl disable oracle-cloud-agent
systemctl stop oracle-cloud-agent-updater
systemctl disable oracle-cloud-agent-updater

# 停止firewall并禁止自启动
systemctl stop firewalld.service
systemctl disable firewalld.service
```

__配置密码登录__

```shell
# 配置root密码
sudo passwd root

# 修改sshd_config配置
vim /etc/ssh/sshd_config

PermitRootLogin yes
PasswordAuthentication yes

# vim end

sudo service sshd restart

```

__通过脚本修改__

```bash
echo root:你的密码 |sudo chpasswd root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo service sshd restart
```

---

``` bash
# debian 10  (-firmware 额外驱动支持, 默认密码MoeClub.org)
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -p 'password' -d 10.3 -v 64 -a -firmware

# ubuntu 20.04
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -p 'password' -u 20.04 -v 64 -a -firmware
```

