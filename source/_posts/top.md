---
title: Top 命令
date: 2025-12-07 14:55:41
tags:
    - top
categories: 
    - note
    - linux
---

[__一.常用快捷键__](#key)
[__二.字段意义__](#word)

# <h2 id="key">一.常用快捷键</h2>

```
space - 暂停
shift+p - 根据CPU占用排序
shift+m - 根据内存占用排序
shift+h - 显示线程
c - 显示完整执行命令
```

# <h2 id="word">二.字段意义</h2>

```
load average = 正在用 CPU + 正在等 CPU + 正在等不可中断 IO 的任务数量
load average: 1分钟平均, 5分钟平均, 15分钟平均
```

```
%Cpu(s):  0.7 us,  0.0 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
us - 用户进程
sy - 系统进程
ni - 优先级
id - 空闲
wa - IO等待
hi - 中断
si - 软中断
st - CPU 被宿主机偷走了 小鸡超开情况
```

```
MiB Mem :    964.7 total,    574.8 free,     66.5 used,    323.4 buff/cache
MiB Swap:    975.0 total,    975.0 free,      0.0 used.    761.8 avail Mem

Mem total - 系统内存大小
Mem used - 进程真实使用的内存总和
avail Mem - 剩余可使用内存
buff/cache - 缓存
```

```
 PID   USER      PR       NI       VIRT        RES      SHR    S    %CPU %MEM  TIME+  COMMAND
进程号  用户  调度优先级  优先级  虚拟地址大小  占用内存  共享内存 状态 

PR - 数字越小 -> 优先级越高
NI - 数字越大 -> 越容易被抢占
S - D 状态是负载高但 CPU 不高的元凶
```

|  状态码  | 含义 |
|  ----  | ----  |
|  R  |  Running  |
|  S  |  Sleeping  |
|  D  |  不可中断睡眠（IO）  |
|  I  |  Idle  |
|  Z  |  Zombie  |