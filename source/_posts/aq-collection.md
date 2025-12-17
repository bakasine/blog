---
title: 合订本
date: 2099-1-1 11:11:11
tags:
    - qa
categories: 
    - qa
---

- [__一. Mac M1 相关问题__](#mac)
  + __{% post_link mac-issue Mac M1 遇到的问题 %}__

- [__二. SSH 相关问题__](#ssh)
  + [1.SSH如何保持连接不自动断开](#ssh_keepalive)
  + [2.SSH使用跳板机](#ssh_jump)
  + [3.SSH滚轮变成浏览历史命令](#ssh_scroll)

- [__三. Linux 相关问题__](#linux)
  + [1.Sed -e expression #1, char 14: unknown option to 's'](#sed_err1)
  + [2.安装应用后提示 Which services should be restarted](#needrestart)
  + [3.解决 vim 使用鼠标选择便进入 visual mode 的问题](#visualmode)
  + __{% post_link top Top 命令 %}__

- [__四. Git 相关问题__](#git)
  + [1.删除Git仓库中的大文件](#git_rm_large_file)
  + [2.加速Git Clone](#clone_speedup)
  + [3.Git Bash乱码问题](#git_bash_code)
  
- [__五. 面试相关问题__](#interview)
  + __{% post_link interview 面试相关 %}__

- [__六. VPS 相关__](#vps)
  + [1.纯 IPv6 怎么访问 IPv4](#vps_ipv6_only)
  + [2.双栈网络设置 IPv4 优先](#vps_ipv4_first)
  + [3.DD系统](#dd)

- [__七. Vim 相关__](#vim)
  + __{% post_link vim Vim 相关 %}__

- [__八. Screen 相关__](#screen)
  + [1.基本用法](#screen_usage)

- [__九. Windows 相关__](#win)
  + [1.关闭更新](#win_upgrade)
  + [2.系统激活](#win_activation)
  + [3.Windows Defender 卸载](#win_defender)
  + [4.缺少 DLL 文件](#win_dll)

# <h2 id="ssh" style="color:#FF8C00">SSH 相关问题</h2>

# <h3 id="ssh_keepalive">1.SSH 如何保持连接不自动断开</h3>

```bash
cat >> ~/.ssh/config << EOF

Host *
    ServerAliveInterval 60
EOF
```

# <h3 id="ssh_jump">2.SSH 使用跳板机</h3>

`1.ProxyJump`

```bash
Host target
 Hostname 
 IdentityFile 
 User 
 Port 
 ProxyJump jump
```

`2.ProxyCommand`

```bash
Host target
 Hostname 
 IdentityFile 
 User 
 Port 
 ProxyCommand ssh jump -W %h:%p
```

# <h3 id="ssh_scroll">3.SSH 滚轮变成浏览历史命令</h3>

windows terminal 使用 git bash 的情况下 -> 注释掉 `/etc/inputrc` 这两行
```
bind '"\e[5~": history-search-backward'
bind '"\e[6~": history-search-forward'
```


# <h2 id="linux" style="color:#FF8C00">Linux 相关问题</h2>

# <h3 id="sed_err1">1.unknown option to 's'</h3>

__sed使用变量替换,且变量含有'/'时__
```bash
var="/etc/host"
// 习惯写法
sed -i "s/regex/$var/" file
// 可用 # ~ 替换
sed -i "s~regex~$var~" file
```

# <h3 id="needrestart">2.安装应用后提示 Which services should be restarted</h3>

`ubuntu默认安装 needrestart 导致`

```bash
apt purge needrestart -y
```

# <h3 id="visualmode">3.解决 vim 使用鼠标选择便进入 visual mode 的问题</h3>

输入 `:set mouse-=a` 或者直接

```bash
cat >> ~/.vimrc <<EOF
:set mouse-=a
syntax on
EOF
```

# <h2 id="git" style="color:#FF8C00">Git 相关问题</h2>

# <h3 id="git_rm_large_file">1.删除Git仓库中的大文件</h3>

```bash
# 找出大文件
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -5 | awk '{print$1}')"

# 重写commit，删除大文件
git filter-branch --force --index-filter 'git rm -rf --cached --ignore-unmatch LARGE_FILE_NAME' --prune-empty --tag-name-filter cat -- --all

# 推送修改后的repo
git push origin master --force

# 清理和回收空间
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
```

# <h3 id="clone_speedup">2.加速Git Clone</h3>

```bash
# 设置 Http Proxy
git config --global http.proxy socks5://127.0.0.1:7890

# 设置 SSH Proxy

vim ~/.ssh/config

# 1.Linux & macOS
cat ~/.ssh/config

Host github.com
 Hostname ssh.github.com
 IdentityFile ~/.ssh/id_rsa
 User git
 Port 443
 #ProxyJump HOST
 #ProxyCommand nc -v -x 127.0.0.1:7890 %h %p
 
# 2.Windows

Host github.com
 Hostname ssh.github.com
 IdentityFile ~/.ssh/id_rsa 
 User git
 Port 443
 #ProxyJump HOST
 #ProxyCommand connect -S 127.0.0.1:7890 %h %p
```

# <h3 id="git_bash_code">3.Git Bash乱码问题</h3>

```bash
cat >> /etc/bash.bashrc << EOF
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
EOF
```

# <h2 id="vps" style="color:#FF8C00">VPS 相关问题</h2>

# <h3 id="vps_ipv6_only">1.纯 IPv6 怎么访问 IPv4</h3>

```bash
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh 4
```

# <h3 id="vps_ipv4_first">2.双栈网络设置 IPv4 优先</h3>

`debian`

```bash
sed -i 's/#precedence ::ffff:0:0\/96  100/precedence ::ffff:0:0\/96  100/' /etc/gai.conf
```

__撤回ipv6优先__

```bash
sed -i 's/precedence ::ffff:0:0\/96  100/#precedence ::ffff:0:0\/96  100/' /etc/gai.conf
```

# <h3 id="dd">3. DD 系统</h3>

__{% post_link re-os DD 系统 %}__

# <h2 id="screen">Screen 相关</h3>

# <h3 id="screen_usage">基本用法</h3>

```bash
# 创建
screen cmd

# 保存退出
ctrl+a d

# ls
screen -ls

# 重新连接
screen -r 编号
```

# <h2 id="win">Windows 相关</h3>

# <h3 id="win_upgrade">关闭更新</h3>

`1.PowerShell管理员模式执行`

```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings" /v FlightSettingsMaxPauseDays /t reg_dword /d 3000 /f
```

`2.去设置修改暂停更新时间`

# <h3 id="win_activation">系统激活</h3>

[Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)

```
irm https://get.activated.win | iex
```

# <h3 id="win_defender">Windows Defender 卸载</h3>

[windows-defender-remover](https://github.com/ionuttbara/windows-defender-remover)

# <h3 id="win_dll">缺少 DLL 文件</h3>

```
sfc /scannow
```