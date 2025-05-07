---
title: oracle免费云服务
date: 2022-09-03 09:46:35
tags:
    - oracle
categories: 
    - vps
---

[注册](#register)
[修改ssh端口密码脚本](#onekey)
[创建实例](#instance)
[申请Ipv6](#ipv6)
[优化系统](#opt)
[dd系统](#dd)
[原邮箱找回](#getback)

# <h2 id="register">注册</h2>

# [oracle](https://www.oracle.com/cloud/sign-in.html)

# <h2 id="onekey">修改ssh端口密码脚本</h2>

```
bash -c "$(curl -L https://cdn.jsdelivr.net/gh/uerax/script@master/ssh.sh)" @
```

# <h2 id="instance">创建实例</h2>

`Launch resources` --> `Create a VM instance` --> `Image and shape` --> `Add SSH keys` --> `Boot volume` --> `Specify a...` 

# <h2 id="ipv6">Ipv6</h2>

`虚拟云网络` -> `点击vcn` -> `CIDR Blocks/Prefixes` -> `Add CIDR Block/IPv6 Prefix` -> __勾选__ `Assign an Oracle allocated IPv6 /56 prefix`

`子网` -> `编辑` -> __勾选__ `Assign an Oracle allocated IPv6 /64 prefix` -> __输入__ `00-FF之间`

`路由表` -> `添加路由规则` -> `选择IPv6`

`安全列表` -> `入站规则` -> `添加入站/出站规则` -> `CIDR | ::/0 | 所有协议`

`附加的VNIC` -> `"IPv6地址` -> `分配IPv6地址` -> `自动或者手动(:ABF)`


# <h2 id="opt">优化系统</h2>

`一键脚本`

```
bash -c "$(curl -L https://cdn.jsdelivr.net/gh/uerax/script@master/ssh.sh)" @
```

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

# <h2 id="dd">dd系统</h2>

``` bash
# debian 11  (-firmware 额外驱动支持, 默认密码MoeClub.org)
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/bakasine/Scripts/main/DebianNET.sh') -d 11 -v 64 -port "2222" -p "密码" 

# ubuntu 22.04
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/bakasine/Scripts/main/DebianNET.sh') -u 22.04 -v 64 -port "2222" -p 'password' 
```

# <h2 id="getback">原邮箱找回</h2>

1.使用原注册邮箱去[support](https://support.oracle.com/)注册并登陆
2.然后创建技术支持工单，会有个选项，选择你的产品，你会发现有个支持id，绑定的是你的oraclecloud计划，详细内容是"我原来的用户因为邮箱损坏，无法继续登陆，麻烦将邮箱重置为xxxx@xxx.xxxx"
3.不出24小时他会告诉你完成，请用新油箱登陆，你去找回密码即可

```
右上角"contact us",然后 点击"Create Non-Technical SR"，
problem type 选择"login/administration/profile issues—login/Assess issue"
Problem Summary里面就写"my oracle account has been stolen"
点击下一步，描述里面就英文写一下"my oracle account has been stolen，please change my administrator email address to XXX@XXX.com"注意这里要一个新邮箱，不能是原邮
然后下一步是传附件之类的，可以传一下邮箱里面账户相关的截图。
然后 提交
我发了不到一个小时就回了，让我提供一个新的邮箱地址，因为我第一次不知道，没提供新的邮箱地址
后来把新邮箱地址 发过去了 ，可能是下班了，目前暂未回消息
```