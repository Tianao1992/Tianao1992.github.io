---
title: 高级运算符
date: 2021-02-04 15:16:46
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 溢出运算符

- swift的运算符出现溢出时会抛出运行时错误

- swift有溢出运算符(&+,&-,&*)

![](/img/ios/swift/class/yunsuanfu.png)

### 运算符重载
枚举、类、结构体、可以为已有的运算符提供自定义实现，这个操作叫运算符重载

```
struct Point {
    var x:Int
    var y:Int
    init(x: Int , y: Int) {
        self.x = x
        self.y = y
    }
    
    //类方法
   static func + (p1: Point, p2: Point) -> Point {
        
        return Point(x: p1.x + p2.x, y: p1.y + p2.y)
    }
    
   // 中缀运算符
    static func - (p1: Point, p2: Point) -> Point {
    
      return Point(x: p1.x -  p2.x, y: p1.y - p2.y)
    }
}
var p1 = Point(x: 10, y: 10)
var p2 = Point(x: 5, y: 5)

let p3  = p1 + p2
```

### Equatable
判断两个实例是否相等，一般遵守Equatable协议，重载 == 运算符

引用类型比较存储地址是否相同
```
class Person : Equatable {
   
    
    var age = 0
    init(age: Int) {
        self.age = age
    }
    static func == (lhs: Person, rhs: Person) -> Bool {
        lhs.age == rhs.age
    }
    
}

var per1 = Person(age: 20)

var per2 = Person(age: 20)

per1 == per2
```

### 自定义运算符

```
prefix operator +++  //前缀运算符
postfix operator -- //后缀运算符
infix operator == //中缀运算符

struct Point {
    var x: Int, y: Int
    static prefix func +++ (p: inout Point) -> Point {
        p = Point(x: p.x + p.x , y: p.y + p.y)
        return p
    }
}
var p = Point(x: 10, y: 20)
+++p
print(p)
```