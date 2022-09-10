---
title: lua-note
date: 2022-09-10 00:43:24
tags:
    - lua
categories: 
    - note
---

[__一.安装lua__](#install)
[__二.基本变量__](#variables)
[__三.运算符__](#operator)
[__四.流程控制__](#processctl)
[__五.函数__](#function)
[__六.String库__](#string)


# <h2 id="install">一.安装lua</h2>

__Mac__

```bash
brew update
brew install lua
```

__Linux__

```bash
sudo apt update && sudo apt install lua5.3
```

__Windows__

[安装包](https://github.com/rjpcomputing/luaforwindows/releases)

# <h2 id="variables">二.基本变量</h2>

```lua
-- local为局部变量, 不做声明默认为全局变量
xint, xfloat = 10, 10.1     --number
local name = "Crayfish Run" --string "" or ''
local names = [[            --string [[字符串块]]
line1
line2
]]
local isAlive = true     --boolean
local a = nil            --no value or invalid value
local talbe = {}         --table
```

__可以使用 type 函数测试变量类型__

```lua
print(type(123))    -- number
print(type('123'))  -- string
print(type(print))  -- function
```

__lua对数字字符进行算术运算的逻辑会将字符串转换成数字进行运算__

```lua
print("2" + 6) -- 8
print("2" * 6) -- 12
print("2" - 6) -- -4
print("-2e2" * "6") -- -1200.0
```

__字符串的连接采用'..'__

```lua
print("2" .. "6") -- 8
print(2 .. 6) -- 8
```

__字符串长度采用#获取__

```lua
print(#'123') -- 3
```

# <h3>table 表</h3>

__表其实就是一种数组+Map,不过和其他语言不同,他的初始index从1开始__

```lua
t = {1,2,3,4,5}                 -- 定义一个表可以看做 [1,2,3,4,5]
t[1]                            -- 1 初始index为1而不是0
t[1] = 2                        -- [2,2,3,4,5]
table.insert(t, 6)              -- 插入6 [2,2,3,4,5,6]
table.insert(t,2,7)             -- 在索引2插入7 [2,7,2,3,4,5,6]
table.remove(t,2)               -- 删除索引2的值 [2,2,3,4,5,6]
table.sort(t)                   -- 升序排序
print(table.concat(t))          -- 所有值连接成string 223456
print(table.concat(t,","))      -- 所有值和分隔符","连接成string 2,2,3,4,5,6
print(table.concat(t,",",2,4))  -- 索引2-4的值分隔符","连接成string 2,3,4
t["key"]="value"                -- 加入后的索引为Key而不是7
```

# <h2 id="operator">三.运算符</h2>

```lua
-- 基础常见不介绍了,只标注和其他语言不一样的点
print(5 // 2)           -- 2    整除(向下取整) 
print(5 ^ 2)            -- 25   乘幂
print(5 ~= 2)           -- true 不等于即 !=
print(true and false)   -- false 即 &&
print(true or false)    -- true 即 ||
print(not true)         -- 逻辑非 取反!
```

# <h2 id="processctl">四.流程控制</h2>

__if__

```lua
if (condition) then
    statement
elseif (condition) then
    statement
else
    statement
end
```

# <h3>循环</h3>

__while__

```lua
-- 条件为真时循环
while (condition) do
    statement
end
```

__for__

```lua
-- 可以看做其他语言的 for i=10; i!=1; i+=-1
-- 即当i不为1时进入循环,每次循环后加上-1. -1可省略默认为1
for i=10,1,-1 do
    statement
end

-- 类似java的foreach, golang的range
-- i为索引,v为值, a为table数组
for i, v in ipairs(a) do
    statement
end
```

__repeat__

```lua
-- java的do while
-- 即while的至少执行一次模式
repeat
    statement
until (condition)
```

# <h3>goto语句</h3>

```lua
-- goto 和其他语言差不多,不同点在于其他语言为 "label:", lua为 "::lable::"
-- 一般用于双循环跳出
local a = 1
::label:: print("--- goto label ---")
a = a+1
if a < 3 then
    goto label -- a 小于 3 的时候跳转到标签 label
end
```

# <h2 id="function">五.函数</h2>

``` lua
function name(param)
    statement
end

-- 函数可以赋值给变量
func = function(param)
    statement
end

-- 不定参数
function name(...)
    -- select("#",...) 可获得参数数量
    statement
end
```

# <h2 id="string">六.String库</h2>


