---
title: Git学习
date: 2018-10-15 00:58:23
cover: /images/preview6.jpg
coverWidth: 1600
coverHeight: 900
tags:
    - git
categories: 
    - note
---

> Login

``` python
git config --global user.name "username"
git config --global email.name "email"
```

> 初始化一个Git仓库

``` python
git init
```

> 添加文件到Git仓库

``` python
//可以用git add .直接将所有文件放入暂存区
git add <file-name>

//提交到版本库, 描述本次提交的说明
git commit -m "本次提交的说明"
```

> 获取工作区的状态

``` python
//查看是否有文件进行修改
git status

//查看修改内容
git diff
```

> 版本控制

``` python
//查看提交日志
git log
//
git log --graph --pretty=oneline --abbrev-commit

//HEAD代表当前版本, HEAD^是上个版本, HEAD^^上上个版本, HEAD~N前N个版本
//也可以用版本号指定

git reset --hard <version>
//由于使用reset回退版本后之前版本的提交日志也会消失
//可以显示你每次命令可以查看到被你回退的版本号
git reflog
```

> 撤销修改

``` python
//让文件回到最近一次git add或者git commit的状态
git checkout -- <file-name>

//将文件移出暂存区, 即取消git add操作
git reset HEAD -- <file-name>
```

> 删除文件

``` python
//与git add的用法一样
//从版本库里中删除文件
git rm <file-name>
git commit -m "本次提交的说明"
```

> 远程仓库

``` python
//添加远程仓库
//origin是远程仓库的默认名称可更改
git remote add origin git@github.com:<github_username>/<repository>

//第一次推送加上-u参数, Git会将本地 master 分支和远程 master 分支关联起来, 以后在推送时可简化命令
git push -u origin master
//推送分支
git push origin <branch-name>

//将远程仓库中的改动同步到本地
git pull

//如果git pull提示no tracking information, 则说明本地分支和远程分支的链接关系没有创建
git branch --set-upstream-to <branch-name> origin/<branch-name>

//查看远程仓库信息, 添加-v常熟可显示更详细信息
git remote
```

> 分支

``` python
//创建分支, 并且切换到该分支
git branch <branch-name>
git checkout <branch-name>
//-b参数相当于上面两条命令
git checkout -b <branch-name>

//在本地创建和远程分支对应的分支, 本地和远程分支的名称最好一致
git checkout -b <branch-name> origin/<branch-name>

//合并指定分支到当前分支
git merge <branch-name>

//删除分支
git branch -d <branch-name>
//如果当前分支还未合并则需要-D参数强行删除
git branch -D <branch-name>
```

> 工作暂存

``` python
//将未提交的修改（包括暂存的和非暂存的）都保存起来
git stash
//推荐使用可添加说明方法
git stash save  "本次提交的说明"

//将缓存堆栈中的stash应用到当前工作目录下, 不删除stash
git stash apply <stash>

//移除stash
git stash drop <stash>

//将缓存堆栈中的第一个stash删除, 并将对应修改应用到当前工作目录下
git stash pop

//查看现有的stash
git stash list
```

> 标签

``` python
//新建一个标签，不写<commit-id>则默认为HEAD
git tag <tag-name> <commit-id>
//可以用参数-a指定标签名，-m指定说明文字
git tag -a <tag-name> -m "说明" <commit-id>

//查看标签信息和说明文字, 按字母排序
git show <tag-name>

//删除本地标签
git tag -d <tag-name>
//删除远程仓库标签
git push origin :refs/tags/<tag-name>

//将标签推送到远程仓库, 可以用--tags参数代替<tag-name>直接将所有标签推送到远程仓库
git push origin <tag-name>
```

> 创建别名

``` python
//将命令名用一个<new-name>代替作为别名
//--global参数使该命令作用在当前用户, 如不加则只针对当前仓库
git config --global alias.<new-name> 'old-name'
//也可通过修改.git/config文件
//在[alias]后面添加
<new-name> = <old-name>

//删除别名
//直接修改.git/config文件
```

> 忽略特殊文件

``` python
Git为我们提供了.gitignore文件，此文件专门来做忽略，只要在此文件中设置你想忽略的内容就可以了。
不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。
所有配置文件可以直接在线浏览：https://github.com/github/gitignore
```

> 下载远程仓库

``` python
git clone <URL>
```