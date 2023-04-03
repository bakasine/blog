---
title: 找到B站下架视频历史弹幕
date: 2023-03-27 21:36:54
tags:
	- danmu
categories: 
    - others
---

## 起因 

__由于B站大部分老番都已下架,有时候想去回顾老番但是没有弹幕又看不下去__
__在国内一顿搜索发现很多百度资源早都过期,而且也没有一个系统性的查找方案__
__最近有去探索一番得到了不少更好的方案, 也看到很多人需要这样的方案__
__所以写出来跟大家分享一下__
  
## 下载方式

### OneDrive网盘下载

[__弹幕下载链接__](https://1drv.ms/f/s!AhuigvBDUdclcjDAVD6xf3b88sg?e=Ki88he)

__缺点:__ 资源较少,只有少数动漫

__来源:__ 大佬在[NGA](https://ngabbs.com/read.php?tid=14925426) 上传的历史弹幕

### 弹幕盒子搜索下载

[__弹幕盒子__](https://danmubox.github.io/search)

__缺点:__ 虽然资源全了很多,但是由于是git page导致国内魔法才能访问

__来源:__ [弹幕保存计划](https://tieba.baidu.com/f?kw=%B5%AF%C4%BB%B1%A3%B4%E6%BC%C6%BB%AE&fr=ala0&tpl=5&dyTabStr=MCw2LDEsMyw1LDQsMiw3LDgsOQ%3D%3D)

有兴趣的可以反代一下或者搭建

## 弹弹play

[弹弹play](https://www.dandanplay.com/)

__缺点:__ 要下载app,不过用potplayer也要下载,但是我不太喜欢下载所以是缺点

## 使用方式

### PotPlayer

__把下载的XML文件转换成ASS文件__ [转换链接](https://danmubox.github.io/convert)

`参数选项` -> `字幕` -> `其他` -> 勾选`当存在两个以上字幕语言时同时输出次字幕语言`

__缺点:__ 需要先下载视频

### 油猴脚本

[脚本链接](https://greasyfork.org/zh-CN/scripts/455196-sakuradanmaku-%E6%A8%B1%E8%8A%B1%E5%BC%B9%E5%B9%95)

__大佬写的支持樱花动漫和其他一些网站的弹幕导入脚本, 配合下载下来的弹幕实现在线观看__
__油猴都不了解的可以自行搜索, 实在不行就是用potplayer方案__

__缺点:__ 支持的网站太少, 基本都是动漫网站缺少日剧等其他资源