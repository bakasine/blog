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

- [__三. Linux 相关问题__](#linux)
  + [1.Sed -e expression #1, char 14: unknown option to 's'](#sed_err1)
  + [2.安装应用后提示 Which services should be restarted](#needrestart)
  + [3.解决 vim 使用鼠标选择便进入 visual mode 的问题](#visualmode)

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

```
Host target
 Hostname 
 IdentityFile 
 User 
 Port 
 ProxyJump jump
```

`2.ProxyCommand`

```
Host target
 Hostname 
 IdentityFile 
 User 
 Port 
 ProxyCommand ssh jump -W %h:%p
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

```
apt purge needrestart -y
```

# <h3 id="visualmode">3.解决 vim 使用鼠标选择便进入 visual mode 的问题</h3>

输入 `:set mouse-=a` 或者直接

```
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
 IdentityFile 
 User git
 Port 443
 ProxyCommand nc -v -x 127.0.0.1:7890 %h %p
 
# 2.Windows

Host github.com
 Hostname ssh.github.com
 IdentityFile 
 User git
 Port 443
 ProxyCommand connect -S 127.0.0.1:7890 %h %p
```

# <h3 id="git_bash_code">3.Git Bash乱码问题</h3>

```
cat >> /etc/bash.bashrc << EOF
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
EOF
```

# <h2 id="vps" style="color:#FF8C00">VPS 相关问题</h2>

# <h3 id="vps_ipv6_only">1.纯 IPv6 怎么访问 IPv4</h3>

```
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh 4
```

# <h3 id="vps_ipv4_first">2.双栈网络设置 IPv4 优先</h3>

`debian`

```
sed -i 's/#precedence ::ffff:0:0\/96  100/precedence ::ffff:0:0\/96  100/' /etc/gai.conf
```

__撤回ipv6优先__

```
sed -i 's/precedence ::ffff:0:0\/96  100/#precedence ::ffff:0:0\/96  100/' /etc/gai.conf
```

# <h3 id="dd">3. DD 系统</h3>

__{% post_link re-os DD 系统 %}__

# <h2 id="screen">Screen 相关</h3>

# <h3 id="screen_usage">基本用法</h3>

```
# 创建
screen cmd

# 保存退出
ctrl+a d

# ls
screen -ls

# 重新连接
screen -r 编号
```