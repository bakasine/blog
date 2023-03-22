---
title: 简单的自动抢购脚本编写方法
date: 2021-08-19 19:19:39
cover: /images/preview3.jpg
coverWidth: 1600
coverHeight: 900
tags:
	- linux
	- shell
	- nodejs
categories: 
    - linux
---

## 事先准备

* Node.js/Python
* postman

## 获取request

```
F12 - network - 右键copy - copy as cURL
```

## 转成js/py脚本

```
postman - 左上角import - row test - import - 点击右边code - 选择 nodejs-request 或者 python-request
```

![1](auto-script/file.png)

## 需要安装request依赖

``` bash
npm install -save request
```

## 编写shell脚本并发执行

```bash
#!/bin/bash

for(( i = 0; i < 200; i++));
do
{
    node ./auto.js >> ./out.txt
    sleep 0.1
}&
done
wait
```

## 使用crontab定时执行脚本
```bash
#每分钟执行一次
* * * * * myCommand

#每天中午12点执行
0 12 * * * myCommand

#每小时的第3和第15分钟执行
3,15 * * * * myCommand

#在上午8点到11点的第3和第15分钟执行
3,15 8-11 * * * myCommand

#每隔两天的上午8点到11点的第3和第15分钟执行
3,15 8-11 */2  *  * myCommand

#每周一上午8点到11点的第3和第15分钟执行
3,15 8-11 * * 1 myCommand

#每晚的21:30重启smb
30 21 * * * /etc/init.d/smb restart

#每月1、10、22日的4 : 45重启smb
45 4 1,10,22 * * /etc/init.d/smb restart

#每周六、周日的1 : 10重启smb
10 1 * * 6,0 /etc/init.d/smb restart

#每天18 : 00至23 : 00之间每隔30分钟重启smb
0,30 18-23 * * * /etc/init.d/smb restart

#每星期六的晚上11 : 00 pm重启smb
0 23 * * 6 /etc/init.d/smb restart

#每一小时重启smb
0 */1 * * * /etc/init.d/smb restart

#晚上11点到早上7点之间，每隔一小时重启smb
0 23-7/1 * * * /etc/init.d/smb restart
```
