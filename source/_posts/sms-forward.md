---
title: 备用手机短信转发方案
date: 2023-04-07 14:55:41
tags:
	- sms
categories: 
    - others
---

## 一. 起因

__由于越来越多账号不支持国内手机和GV注册__
__所以最近买了张免年费的国外SIM卡来使用__
__但是卡一多问题就出来了,出门不爱带包两个手机踹口袋裤子都要掉了__
__所以不得不找个方案, 让我出门只需要带一个手机__

## 二. Android 备用机的转发方案

### 1.SmsForwarder + Telegram Bot

[SmsForwarder](https://github.com/pppscn/SmsForwarder)

__SmsForwarder是个Github上的开源库,支持监控Android手机短信、来电、APP通知并转发__
__同时也包括远程控制发短信发短信、查短信、查通话、查话簿、查电量等功能__

__这边根据官方文档给出一个简单的搭建流程,如果不想使用Telegram Bot可以去看[文档](https://github.com/pppscn/SmsForwarder/wiki/2.%E5%8F%91%E9%80%81%E9%80%9A%E9%81%93)自行配置__

> 通用设置
* 按需打开转发功能的总开关，会弹出必需的权限授权；如果授权不正常，请去手机的【设置】中手动设置权限（无脑全部授予）
* 保活措施建议开启前3项设置
* 个性设置中卡槽备注点击刷新自动获取，如果转发信息中的卡槽匹配错误，根据SubId设置卡槽主键
* 如果设备处在网络不稳定的环境，请设置请求重试机制的重试次数

> 发送通道

__我是用Telegram作为转发的工具,也可以使用SMS或者邮箱之类的__

* 申请Telegram Bot

```
与 @BotFather 私聊，申请 Bot
发送/newbot 后输入机器人昵称
然后输入机器人的用户名
/token 获取apiToken，然后输入上面机器人的用户名
获得apiToken，格式参考：1234567890:ABCDEFGHIJKLMNOPQRSTUVWXYZ
复制 apiToken 到「设置Telegram机器人的ApiToken」一栏
跟自己的机器人聊天，随便说点什么；或者创建一个群组，把机器人拉入群组，在群组里随便说点什么。
然后打开这个链接 https://api.telegram.org/bot<apiToken>/getUpdates 获取（PS.注意<apiToken>整个换成你自己的）
ChatID 取值 result->message->chat->id (个人是纯数字；群组是负数，type：group；)
获取自己（或群组）的ChatID，粘贴到「设置被通知人的ChatId」一栏
点击【测试】按钮验证一下
```

> 通话转发规则

* 发送通道选择刚刚添加的Telegram Bot
* 执行逻辑 -> 成功即止
* 匹配字段 -> 全部
* 启用该条转发规则

__然后就可以发一条短信进行测试,如果有问题那就看文档或者自己Google__

### 2. Tasker + Telegram Bot

> 注: Tasker是收费App

* 申请Telegram Bot

```
与 @BotFather 私聊，申请 Bot
发送/newbot 后输入机器人昵称
然后输入机器人的用户名
/token 获取apiToken，然后输入上面机器人的用户名
获得apiToken，格式参考：1234567890:ABCDEFGHIJKLMNOPQRSTUVWXYZ
复制 apiToken 到「设置Telegram机器人的ApiToken」一栏
跟自己的机器人聊天，随便说点什么；或者创建一个群组，把机器人拉入群组，在群组里随便说点什么。
然后打开这个链接 https://api.telegram.org/bot<apiToken>/getUpdates 获取（PS.注意<apiToken>整个换成你自己的）
ChatID 取值 result->message->chat->id (个人是纯数字；群组是负数，type：group；)
获取自己（或群组）的ChatID，粘贴到「设置被通知人的ChatId」一栏
点击【测试】按钮验证一下
```

* 创建 Task

__添加一个 HTTP Request 动作:__

Method 选 POST
URL 一栏填写：https://api.telegram.org/bot<你的TOKEN>/sendMessage
Headers 一栏填写：Content-Type:application/json （可以点击放大镜快速选择）
Body内容填写如下（记得chat_id替换为你的uid）：
```
{
    "chat_id": <YOUR_CHAT_ID>,
    "parse_mode": "HTML",
    "text": "<b>%SMSRF(%SMSRN)</b> \n\n%SMSRB\n\n 时间：%SMSRD"
}
```
其中用到了几个 Tasker 自带的变量：
>%SMSRF：sender address 地址
%SMSRN：sender name 通讯录中的名称或号码
%SMSRB：主体（短信内容）
%MMSRS：主题（一般彩信才有）
%SMSRD：接收日期
%SMSRT：接收时间

* 创建 Profile 来调用 Tasker

切换到 Tasker 的 PROFILES 选项卡，添加一个 Event 类型的 `Profile ：Phone > Received Text`，按需求配置是否需要过滤类型，发送者和内容。

创建之后选择链接到刚刚创建的 Task就完成了。

## 三. Iphone 备用机的转发方案

__iphone应用默认是没权限读取短信内容,然后快捷指令自动化还强制必须指定关键词或者联系人,暂时没找到转发给Android的方式__

### 1. 转发到Iphone

```
在iPhone上启动设置
转到消息
切换iMessage
查找并点按短信转发
找到想要接收和发送短信的 iOS 设备(只有同一个apple id的设备才会显示在里面)
验证码将发送到请求的设备
```

__没有两台iphone没法测试, 看有些大佬反馈不同wifi下同步会有问题, 所以备用机还是用Android吧__