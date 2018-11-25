---
title: "Go基础：顺序编程"
date: 2018-11-25T21:23:18+08:00
archives: "2018"
tags: ["go"]
author: Gilfoyle.Song
---
### 变量

```go
//变量声明
var v1 int
var v2 string
var v3 [10]int  //array
var v4 []int //slice
var v5 struct{
    f int
}
var v6 *int  //pointer
var v7 map[string]int //map,key:string ,value:int
var v8 func(a int) int

var(
	v1 int
    v2 string
)
//变量初始化
var v1 int=10
var v2 =10
v3:=10

//变量赋值

var v10 int
v10=123

i,j=j,i
```

### 常量

```go
//字面常量：程序中硬编码的常量
-12
"foo"
true
//常量定义
const pi float64=3.1415124141
const zero=0.0
const(
	size int64 =1024
    eof=-1
)
const u,v float32=0,3
const a,b,c=3,4,"foo"
//常量定义的右值可以是一个在编译期运算的常量表达式：
const mask =1<<3
// 注意：常量的赋值是一个编译期行为，所以 值不能出现任何需要 行期才能得出结果的表达式


//预定义常量
Go语言预定义了这些常量：true、false、iota。iota在每一个const关键字出现时重置为0，然后在下一次const出现之前，每出现一次iota，其代表的数字会自动增1.

//枚举
const(
	Sunday=iota
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    numberOfDays //
)
```

### 类型

Go语言内置以下这些基础类型：

- [ ] 布尔类型：bool
- [ ] 整型：int8、byte、int16、int、uint、uintptr等
- [ ] 浮点类型：float32、float64.
- [ ] 复数类型：complex64、complex128
- [ ] 字符串：string
- [ ] 字符类型：rune
- [ ] 错误类型：error

此外，Go 语言也支持以下复合类型：

- [ ] 指针（pointer）
- [ ] 数组（array）
- [ ] 切片（slice）
- [ ] 字典（map）
- [ ] 通道（chan）
- [ ] 结构体（struct）
- [ ] 接口（interface）





### 错误处理

#### 1.error接口

Go语言引入了一个关于错误处理的标准模式，即error接口，该接口的定义如下：

```go
type error interface {
    Error() string
}
```

​	我们可以实现error接口，自定义错误。



#### 2. defer

​	类似于C++里面的析构函数用于释放资源。一个函数中可以存在多个defer语句，因此我们要注意defer语句的调用遵照先进后出的原则，即最后一个defer语句将最先被执行。



#### 3.panic()和recover()

​	Go语言引入了两个内置函数`panic()`和`recover()`来报告和处理运行时错误和程序中的错误场景：

```go
func panic(interface{})
func recover() interface{}
```

​	当在一个函数执行过程中调用`panic()`函数时，正常的函数执行流程将立即终止，但函数中之前使用`defer`关键字延迟执行的语句将正常展开执行，之后该函数将返回到调用函数，并导致逐层的向上执行`panic`流程，直至所属的goroutine中所有正在执行的函数被终止。错误信息将被上报，包括在调用panic()函数时传入的参数，这个过程称之为错误处理流程。

​	recover()函数用于中止错误处理的流程。一般，recover()应该在一个使用defer关键字的函数中执行以有效截取错误处理流程。如果没有在发生异常的goroutine中明确调用recover关键字的恢复过程，会导致该goroutine所属的进程打印异常信息后直接退出。

```go
defer func(){
    if r:=recover();r!=nil{
        log.Printf("Runtime error caugth:%v",r)
    }
}
foo()
```

​	无论foo()中是否出发了错误处理流程，该匿名函数defer函数都将在函数退出时得到执行。假如foo中触发了错误处理流程，recover函数将使得该错误处理过程终止。如果错误处理流程被触发时，程序传给panic的参数不为空，则该函数会打印详细的错误信息。


