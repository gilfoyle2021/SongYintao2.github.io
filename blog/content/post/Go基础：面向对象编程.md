---
title: "Go基础：面向对象编程"
date: 2018-11-25T21:18:46+08:00
archives: "2018"
tags: ["go"]
author: Gilfoyle.Song
---
如果想要改变变量的值，只能传递指针。

### 类型系统

#### 1.值语意、引用语义

​	**值语意**：

```go
b=a
b.Modify()
```

如果b的修改不会影响a的值，那么这个类型属于值类型。如果会影响a的值，那么此类型为引用类型。

#### 2.结构体

```go
type Rect struct{
    x,y float64
    width,height float64
}
//define a member method to cal area
func (r *Rect) Area() float64{
    return r.width*r.height
}
```

#### 3.初始化

```go
rect1:= new (Rect)
rect2:=&Rect{}
rect3:=&Rect{0,0,100,200}
rect4:=&Rect{width:100,heigth:200}


func NewRect(x,y,width,height float)
```

#### 4. 匿名组合

```go
type X struct{
    Name string
}

type Y struct{
    X
    *log.Logger
    NNN string
}
```

### 接口

#### 1. 接口查询

```go
var file Writer=...
if file5,ok:=file1.(two.IStream);ok{
    ...
}

```

#### 2.类型查询

```go
var v1 interface{}=,,,
switch v:=v1.(type){
    case int:
    case string:
    ...
}
```

#### 3.接口组合

```go
type ReadWriter interface{
    Reader
    Writer
}
//这个接口组合了Reader和Writer两个接口
```

#### 4.Any类型

​	由于Go语言中任何对象实例都 足 接口interface{}，所以interface{}看起来 是可以指向任何对象的Any类型。

```go
var v1 interface{}=1  //int 赋值给interface{}
var v2 interface{}="abc"
var v3 interface{}=&v2 //将*interface{} 赋值给interface{}
var v4 interface{}=struct{X int}{1}
var v5 interface{}=&struct{X int}{1}
...

func Printf(args ...interface{})
...

```


