---
title: Go笔记(2)
date: 2021-09-29 01:59:25
tags:
    - go
categories:
    - go
---

* [Go变量声明和初始化](#variable)
* [Go作用域](#scope)

### <h2 id="variable">Go变量声明和初始化</h2>

1. Go与Java不同，声明变量的类型在变量的名称之后
```go
var name type
var i int = 1
```

2. Go的基本类型有

```properties
bool // true | false
string // for循环取出rune类型
int // 根据操作系统位数决定是int32还是int64
int8 // 占1个字节 【-1 * 2 ^ 7，1 * 2 ^ 7 - 1】7是因为还有1位要作为符号位，-1是补码需要，最小值不需要-1因为还有个-0可以使用
int16 // 占2个字节 【-1 * 2 ^ 15，1 * 2 ^ 15 - 1】
int32 // 同
int64 // 同
uint // 根据操作系统位数决定是int32还是int64
uint8 // 无符号整数，即非负数 1个字节 【0，1*2^8 - 1】 8是因为不需要符号位
uint16、uint32、uint64 // 同上
uintptr // 用uint保存地址
byte // uint8 的别名
rune // int32 的别名 代表一个 Unicode 码
float32、float64
complex64、complex128
```

3. 批量格式

```go
var (
	name1 type1
	name2 type2
)
```

4. 简短格式

* 定义变量，同时显式初始化。
* 不能提供数据类型。
* 只能用在函数内部。

``` golang
// 名字 := 表达式
i, j := 0, 1
x := 100
a, s = 1, ""
```

5. 编译器推导

```
var name
var x = 100
```

### <h2 id="scope">Go作用域</h2>

1. 变量作用域

``` golang
v := 0
if condition {
	v := 1
	fmt.Println(v) // 1
}
fmt.Println(v) // 0
// if for 里的变量都只作用于他们块中
```