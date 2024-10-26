---
title: Vim 相关
date: 2024-03-12 02:44:26
tags:
    - vim
categories: 
    - note
---

```
viw // 选中单词

CTRL + O  // 光标跳转到之前的位置

v%d // 复制整个括号的内容，需要用p粘贴

daw // 删除光标所在的单词
di" // 删除双引号内的文本
d% // 删除整个括号内容，光标必须指向括号

ctrl+v I 注释 ESC // 批量注释
ctrl+v d // 批量取消注释
 
gg // 跳转顶部
G // 跳转底部

u // 撤回操作
. // 重复上个操作

/ // 搜索 n 下一项 N 上一项
:g/  // 仅显示搜索匹配的项目

gg=G // 格式化

:set nu // 显示行号
:set nonu // 不显示行号

"+y // 复制到粘贴板，必须 vim --version | grep clipboard 显示 +clipboard

yy // 复制行
dd // 剪切行
p // 粘贴行

:wq // 保存并退出。
ZZ  // 保存并退出。
:q! // 不保存并退出。

k j h l // 上 下 左 右
```