---
title: Go笔记(1)
date: 2021-09-29 00:08:42
tags:
    - go
categories:
    - go
---

* [Go安装](#install)
* [Go工程结构](#struct)
* [Go程序的编译和运行](#compile)

### <h2 id="install">Go安装</h2>

1. Windows 下的安装步骤

- [下载地址](https://golang.google.cn/dl/)

- 安装 msi 文件

- 配置环境变量 GOPATH，GOPATH 是一个路径，用来存放开发中需要用到的代码包
	
1. Linux 下的安装步骤

- [下载地址](https://golang.google.cn/dl/) 

- 配置环境变量

``` bash
wget https://dl.google.com/go/{开发包名}
tar -C {安装路径} -xzf {开发包名}
vi /etc/profile
	export GOROOT={gopath}
	export PATH=$PATH:$GOROOT/bin:$GOBIN
```

3. Mac 下的安装步骤

- [下载地址](https://golang.google.cn/dl/)

- 安装 pkg 文件（如果是M1版本需要下载arm的安装文件）

- 配置环境变量

``` bash
vi ~/.bash_profile
	export GOPATH=$HOME/go
	source ~/.bash_profile
	export GOROOT=/usr/local/go
```

### <h2 id="struct">Go工程结构</h2>

一个Go语言项目的目录一般包含以下三个子目录：
* src 目录：放置项目和库的源文件；
* pkg 目录：放置编译后生成的包/库的归档文件；
* bin 目录：放置编译后生成的可执行文件。

手动在 GOPATH 中创建以上三个文件夹，并在 src 下创建不同的项目文件夹

### <h2 id="compile">Go程序的编译和运行</h2>

``` go
// build 编译成二进制的可执行文件
go build file.go

// install 一是编译包文件（无main包），将编译后的包文件放到 pkg 目录下（$GOPATH/pkg）。二是编译生成可执行文件（有main包），将可执行文件放到 bin 目录（$GOPATH/bin）
go install file.go

// run 直接执行
go run file.go

```
