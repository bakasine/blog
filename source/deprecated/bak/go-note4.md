---
title: Go笔记(3)
date: 2021-12-08 01:42:11
tags:
    - go
categories:
    - go
---

* [Go协程](#goroutine)
* [Go结构体](#struct)
* [Go接口](#interface)

### <h2 id="goroutine">Go协程</h2>



``` go
// 通过协程运行方法
go method()

// channel 协程的通信机制
ch := make(chan int) // 无缓存channel接收int
ch <- val // val 传入 ch， 无缓存Channels的发送操作将导致发送者goroutine阻塞，直到另一个goroutine执行接收操作
val = <- ch // val 接收 ch
<- ch // 丢弃结果

// 关闭一个 channel
close(ch)



```