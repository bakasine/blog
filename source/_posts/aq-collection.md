---
title: 开发遇见的问题合集
date: 2099-1-1 11:11:11
tags:
    - qa
categories: 
    - qa
---

- [__一. SSH 相关问题__](#ssh)
  + [1.SSH如何保持连接不自动断开](#ssh_keepalive)

- [__二. Linux 相关问题__](#linux)
  + [1.Sed -e expression #1, char 14: unknown option to 's'](#sed_err1)

- [__三. Mac M1 相关问题__](#mac)

- [__四. Git 相关问题__](#git)
  + [1.删除Git仓库中的大文件](#git_rm_large_file)
  + [2.加速Git Clone](#clone_speedup)

# <h2 id="ssh">SSH 相关问题</h2>

# <h3 id="ssh_keepalive">1.SSH 如何保持连接不自动断开</h3>

```bash
cat > ~/.ssh/config << EOF
Host *
    ServerAliveInterval 60
EOF
```

# <h2 id="linux">Linux 相关问题</h2>

# <h3 id="sed_err1">1.unknown option to 's'</h3>

__sed使用变量替换,且变量含有'/'时__
```bash
var="/etc/host"
// 习惯写法
sed -i "s/regex/$var/" file
// 可用 # ~ 替换
sed -i "s~regex~$var~" file
```

# <h2 id="mac">Mac M1 相关问题</h2>

__{% post_link mac-issue Mac M1 遇到的问题 %}__

# <h2 id="git">Git 相关问题</h2>

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
 IdentityFile /xxx/.ssh/github_id_rsa
 User git
 Port 443
 ProxyCommand nc -v -x 127.0.0.1:7890 %h %p
 
# 2.Windows

Host github.com
 Hostname ssh.github.com
 IdentityFile /c/users/xxx/.ssh/github_id_rsa
 User git
 Port 443
 ProxyCommand connect -S 127.0.0.1:7890 %h %p
```
