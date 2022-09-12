---
title: lua笔记
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
[__七.模块__](#module)
[__八.元表__](#metatable)
[__九.协程__](#coroutine)
[__十.文件IO__](#io)
[__十一.面向对象__](#object)
[__十二.错误处理__](#error)


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
local names = [[            
    line1
    line2
]]			 --string [[字符串块]]
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

```lua
string.upper("a") -- A
string.lower("A") -- a
string.gsub("aaaa", "a", "c", 3) -- ccca 把aaaa的前三个a替换成c, 3可省略, 省略为全部替换
string.find("abcde", "bc", 1) -- 2 3 查找abcde的bc索引位置,从索引1开始查找. 1可省略,默认为从头开始查找
string.reverse("12345") -- 54321
string.format("xxx:%d", 1) -- xxx:1
string.byte("A")    -- 65   多个值取第一个
string.char(65)     -- A    多个值连接
string.len("123")   -- 3
string.rep("123", 2)    -- 123123 复制2个123并连接
```

# <h2 id="module">七.模块</h2>

__自定义模块__

```lua
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "const"
 
-- 定义一个函数
function module.func1()
    print("func1")
end

-- local私用化声明
local function func2()
    print("这是一个私有函数！")
end

function module.func3()
    func2()
end
 
return module
```

__加载模块__

```lua
local m = require("module")
print(m.constant)
m.func1()
```

__加载路径默认为LUA_PATH,需要手动配置__

# <h2 id="metatable">八.元表</h2>

__相当于重写表的基本操作函数__

```lua
-- table原生不支持 +,- 等操作,需要通过元表
-- 声明元表
local mt = {}

-- 对元表添加_add方法，用于描述+法操作
mt.__add = function(a, b)
    local res = {}
    
    statement

    return res
end

t1 = {1,2,3}
t2 = {2,3,4}
setmetatable(t1, mt)
t3 = t1 + t2 -- 3,5,7
```

<h3>支持的元方法</h3>

| `元方法`     | `运算符`     | 
| ----------- | ----------- |
| __add       | +           |
| __mul	      | *           |
| __sub	      | -           |
| __div	      | /           |
| __unm		  | !           |
| __mod		  | %           |
| __pow	      | ^           |
| __concat	  | ..          |
| __eq        | ==          |
| __lt        | <           |
| __le        | <=          |
| __tostring  | 输出字符串    |
| __call	  | 函数调用      |
| __index	  | 调用索引值    |
| __newindex  | 赋值         |

# <h2 id="coroutine">九.协程</h2>

```lua
-- 创建coroutine
co = coroutine.create(
    function(i)
        print(i)
    end
)

coroutine.status()        -- 查看 coroutine 的状态 dead，suspended，running
-- 创建 coroutine，返回一个函数，一旦你调用这个函数，就进入 coroutine
cw = coroutine.wrap(
    function(i)
        print(i)
    end
)
cw(1)                   -- 1

-- 需要在 coroutine的方法中,可以使其挂起.
coroutine.yield()
coroutine.resume(co, 1)   -- 重启 coroutine
-- 返回正在跑的 coroutine，一个 coroutine 就是一个线程，当使用running的时候，就是返回一个 corouting 的线程号
coroutine.running()

```

# <h2 id="io">十.文件IO</h2>

| __参数__     | __效果__     | 
| ----------- | ----------- |
| r |	以只读方式打开文件，该文件必须存在。|
|w	|打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。|
|a	|以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾|
|r+	|以可读写方式打开文件，该文件必须存在。|
|w+	|打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。|
|a+	|与a类似，但此文件可读可写|
|b	|二进制模式，如果文件是二进制文件，可以加上b|
|+	|号表示对文件既可以读也可以写|

```lua
-- 只读
file = io.open("file", "r")
-- 输出文件第一行
print(file:read())

file:flush() -- 刷新

-- 关闭打开的文件
file:close()

-- 以附加的方式打开只写文件
file = io.open("test.lua", "a")

-- 在文件最后一行添加 Lua 注释
file:write("--test")
```

__read()的参数__

| __参数__     | __效果__     | 
| ----------- | ----------- |
| a       | 读取文件全部内容   |
| l	      | 表示读取一行，不带换行符           |
| L	      | 表示读取一行，带换行符           |
| n	      | 表示读取一个数字     |
| num     | 表示读取num个字符,num表示数字|

``` lua
-- 读取全部
file:read("a")
```


# <h2 id="object">十一.面向对象</h2>

# <h3>创建类</h3>

```lua
-- lua 中的类可以通过 table + function 模拟出
Clz = {p = 0}
function Clz.paramMinus(v)
    print(Clz.p - v)
end

Clz.paramMinus(10)   -- -10

-- 对象
Object = {param = 0}

-- 派生类的方法 new
function Object:new (o, param)
  o = o or {}   -- 如果 o 为 false 或 nil ，则 o ={} 
  setmetatable(o, self)
  self.__index = self
  self.param = param
  return o
end

-- 基础类方法 printArea
function Object:printP ()
  print(self.param)
end

-- 创建对象
myobj = Object:new(nil,10)
myobj:printP() -- 10

```

`.`和`:`调用的区别在于默认self

```lua
clz = {v=0}
function clz.add(self, v)
    self.v = self.v + v
end
clz.add(clz, v)
-- 上下方法一致
function clz:add(v)
    self.v = self.v + v
end
clz:add(v)

```

# <h3>继承</h3>

```lua
clz = {v=0}
function clz:new(o, v)
    o = o or {}
    metatable(o, self)
    self.__index =self
    self.v = v
    return 0
end

function clz:add(v)
    self.v = self.v + v
end

-- 继承

ext = clz:new(nil,1)
function ext:new(o, v)
    o = o or clz:new(o, v)
    setmetatable(o, self)
    self.__index=self
    return o
```

# <h2 id="error">十二.错误处理</h2>

# <h3>error</h3>

```lua
-- 抛出异常
error("msg")
```
# <h3>assert</h3>

```lua
-- assert是一个断言, 包装error实现. 它会中断当前流程, 可省略抛出信息参数
assert(type(a) == "number", "抛出的错误信息")
```

# <h3>pcall 和 xpcall、debug</h3>

```lua
if pcall(function, ...) then
-- 没有错误
else
-- 一些错误
end

-- 传入第一个值为函数，后面的则为参数
pcall(function(i) print(i) end, 33) -- true 或者 false stdin:1: error..

-- 即java的catch
-- 传入第一个值为函数，第二个为报错函数(自动传入err消息)，后面则为参数

-- debug.traceback：根据调用桟来构建一个扩展的错误消息
xpcall(function(i) print(i) error('error..') end, function() print(debug.traceback()) end, 33)
-- stack traceback: ...    false nil
```



