---
title: Go笔记(3)
date: 2021-12-05 19:08:42
tags:
    - go
categories:
    - go
---

* [Go复合类型](#compound)
* [Go结构体](#struct)
* [Go接口](#interface)

### <h2 id="compound">Go复合类型</h2>

1. Array
	
``` go
var q [3]int
var q [3]int = [3]int{1, 2, 3}
q := [...]int{1, 2, 3}
```

2. Slice

``` go
// slice之间可以共享底层的数据 类似于Java的ArrayList
var s []int    // len(s) == 0, s == nil
s = nil        // len(s) == 0, s == nil
s = []int(nil) // len(s) == 0, s == nil
s = []int{}    // len(s) == 0, s != nil
months := [...]string{1: "January", /* ... */, 12: "December"} // index: value
s := months[4:7]
s := make([]T, len, cap) // same as make([]T, cap)[:len]

// 使用例
s = append(s, subS) // 添加
s = append(s[:del], s[del + 1:]...) // 删除
s[0] = value // 可能会导致其他切片的内容也收到改变

// 遍历
for index, value := range s {
}
```

3. Map

``` go
ages := make(map[string]int) // string:int
ages["alice"] = 31
ages["charlie"] = 34
ages := map[string]int{
    "alice":   31,
    "charlie": 34,
}

// 使用例
delete(ages, “alice”) // 删除
ages["alice"]++ // 加一
_, ok := range ages[key] // key是否存在
```

### <h2 id="struct">Go结构体</h2>

1. 函数和方法

``` go
// 函数 注：go没有重载
func functions() {}
// 调用方法
functions()

// 方法 针对某个结构的方法
type Test struct {
	A int
	B String
}
// Test接收值不为指针的话，无法改变调用方法的结构体的值
func (t *Test) method() {}
// 调用方法
t := &Test{1, ""}
t.method()
```

2. 结构体嵌入

``` go
type House struct {
	Size uint
} 

type Human struct {
	A string
	/* 嵌入扩展
		 当要将Human转成Json时，相当于
		 "human": {
		 	"a":"",
		 	"house": {
		 		"size":""
		 	}
	*/
	house House 
}

type Human struct {
	A string
	/* 嵌入扩展
		 当要将Human转成Json时，相当于
		 "human": {
		 	"A":"",
		 	"Size":""
		 	}
	*/
	House 
}
```

3. 方法表达式

``` go
type Test struct {
	A int
	B String
}
func (t *Test) Method(val int) {}

t := new(Test)

alias := t.Method
alias(val)  // t.Method(t)
```

### <h2 id="interface">Go接口</h2>

```go
// 实现了接口的所有方法，则认为实现了该接口
type Person interface {
  GetName() string
  GetSex
}

type Student struct {
  Name string
  Sex string
}

func (s *Student) GetName() string {
  return s.Name
}

func (s *Student) Sex() string {
  return s.Sex
}

//  判断interface是否实现接口
type Animal struct {}

var t interface{}
t = &Animal{}
_, ok := t.(Person) // false
t = &Student{}
_, ok := t.(Person) // true

// 判断接口类型
switch t.(type) {
case int: // int类型
case string: // string类型
default: // 其他类型
}

//空接口就是不包含任何方法的接口。正因为如此，所有的类型都实现了空接口。
var a interface{}
a = ""
a = 1


// 一个包含nil指针的接口不是nil接口
var p Person
p = &Student{}
notNil := func() *Student { return nil } 
p = notNil() // p != nil 因为interface中的value为nil但type不为nil
isNil := func() *Person { return nil }
p = isNil() // p == nil
```
