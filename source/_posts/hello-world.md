---
title: GitHub + Hexo 搭建博客
date:  2018-05-26 01:59:25
tags: 
  - hexo
  - github
categories: 
  - others
---

## 事先准备

* Node.js
* git

## 创建 GitHub Pages
这个百度都有

## 安装 Hexo 并检查是否安装成功

``` bash
cd D://hexo
npm install hexo -g
hexo -v
```

## 初始化 Hexo

``` bash
hexo init
```

## 依赖包安装

``` bash
npm install
```

## 编译

``` bash
hexo g
```

## 打开服务器

``` bash
hexo s
```

默认是 localhost:4000

## 联系到 GitHub

打开 Hexo 文件夹里的 _config.yml 文件
配置Deployment

``` bash
deploy:
  type: git
  repository: 你的 repo 值
  branch: master
```

## 安装扩展

``` bash
install hexo-deployer-git --save
```

## 写作

``` bash
hexo new <file-name>
hexo n "我的第一篇文章"
```

## 部署到GitHub

``` bash
hexo d
```

## 问题处理

``` bash
// 下载国外的资源众所周知的慢，常用设置镜像，平时多用yarn
// yarn全局安装及设置镜像
npm install -g yarn
yarn config set registry http://registry.npm.taobao.org/ -g
yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
npm config set registry https://registry.npm.taobao.org
npm config get registry // 查看是否配置成功
npm config list  // 查看npm当前配置
npm cache clear --force // 强制清除缓存

```