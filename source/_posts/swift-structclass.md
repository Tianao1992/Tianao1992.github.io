---
title: 结构体和类
date: 2021-02-02 16:44:13
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 结构体

在swift 标准库中，绝大多数的公开类型都是结构体，枚举和类只占一小部分
- 比如 Bool,Int,String,Arrray,Dictionary等常见类型都是结构体

- 所有结构体都有编译器自动生成的初始器(initialer,初始化器，构造器，构造方法)

**初始化器宗旨是：保证每个成员都有值**
```
struct Point {
    var x: Int
    var y: Int
    
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(x:10)//Missing argument for parameter 'y' in call
var p3 = Point(y:20)//Missing argument for parameter 'x' in call
```

#### 自定义初始化器

一旦定义了自定义初始化器，编译器就不会帮它自动生成
```
struct Point {
    var x: Int = 0
    var y: Int = 0
    
    init(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
    
    
}

var p1 = Point(x: 10, y: 10)
var p2 = Point(x:10)//Missing argument for parameter 'y' in call
var p3 = Point(y:20)//Missing argument for parameter 'x' in call
```

### 类

类和结构体类似，但编译其没有帮它自动生成可以传值的初始化器
![](/img/ios/swift/class/classinit.png)

#### 类的初始化器
如果类的所有成员都在定义的时候有初始化值，编译器会自动生成无参的初始化器

```
class Point {//Class 'Point' has no initializers
    var x: Int = 0
    var y: Int = 0
}
var p1 = Point()
```

### 类和结构体的本质

结构体内存在哪，取决于你在哪创建它

函数里在栈 外部在全局 在类里在堆空间

类无论在哪定义 都是在堆空间
![](/img/ios/swift/class/benzi.png)

#### 值类型

值类型赋给var let 或者函数传参是将所有的内容拷贝一份
，类似于产生一个新的副本，属于深拷贝

#### 值类型赋值操作
```
struct Point {
    var x: Int
    var y: Int
}

var p1 = Point(x: 10, y: 20)
p1 = Point(x: 11, y: 22)
```
内存地址没有发生变化，内存数据发生改变

#### 引用类型

引用类型赋值给let,var 或者函数传参只是将内存地址拷贝一份
属于浅拷贝

```
class Size {
    var height: Int
    var width: Int
    init(height: Int, width: Int) {
        self.height = height
        self.width = width
    }
}

func test() {
    var s1 = Size(height: 10, width: 20)
    var s2  = s1
}

```
s1 和 s2 指向同一块内存空间


#### 引用类型赋值操作

```
class Size {
    var height: Int
    var width: Int
    init(height: Int, width: Int) {
        self.height = height
        self.width = width
    }
}

func test() {
    var s1 = Size(height: 10, width: 20)
    s1 = Size(height: 11, width: 22)
}
```
内存地址相同，内存数据指向一块新的内存

#### 类，枚举，结构体都可以定义方法

```
class Size {
    var height = 10
    var width = 20
    func show() {
        print("show")
    }
}
var s1 = Size()
s1.show()

struct Point{
    var x = 2
    var y = 2
    func show(){
        print("point show")
    }
}

var p1 = Point()
p1.show()

enum Dirtion {
    case north, west
    func show() {
        print("dirtion show")
    }
}
var dr = Dirtion.north

dr.show()

```
- 方法不占用对象内存

- 方法本质就是函数

- 函数，方法都存放在代码端




