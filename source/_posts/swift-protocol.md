---
title: 协议
date: 2021-02-04 09:08:47
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---
### 协议

协议可以用来定义方法、属性、下标的声明,协议可以被枚举、类、结构体所遵守
```
protocol Drwawnbale {
    func draw()
    var x: Int {get}
    subscript(index: Int) -> Int{get set}
 }

```
协议中定义方法不能有默认参数值

默认情况下，定义的内容必须全部实现

### 协议中的属性

```
protocol Drwawnbale {
    func draw()
    var x: Int {get}
    var y: Int {get set}
    subscript(index: Int) -> Int{get set}
 }
```
- 协议中定义属性必须是 var 

- 实现协议属性权限不能小于协议中定义属性权限

- 协议定义 get set 用var 实现存储属性或计算属性

- 协议定义 get 任何属性都能实现

```
protocol Drwawnbale {
    func draw()
    var x: Int {get set}
    var y: Int {get }
    subscript(index: Int) -> Int{get set}
 }


class Point : Drwawnbale {
    func draw() {}
    var x:Int = 10
    var y: Int {
        set{}
        get {0}
    }
    
    subscript(index: Int) -> Int {
        get{ index }
        set{}
    }
}

```

### static、class

为保证通用，定义类方法、类属性、类型下标必须用static 

```
protocol Drwawnbale {
   static func test()
 }


class Point : Drwawnbale {
    class func test() {
        
    }
}
```

### mutating

只有将协议的实例方法标记为 mutating

- 才能允许结构体、枚举具体实现自身修改

- 类实现实例方法不同加mutating、 结构体、枚举需要

```
protocol Drwawnbale {
   mutating func test()
 }


class Point : Drwawnbale {
    var x: Int = 10
    func test() {
        x += 10
    }
}

struct Person: Drwawnbale {
    var x: Int = 0
    mutating func test() {
        x += 10
    }
}
```

### init

协议中还可以定义init,非final 必须加required
```
protocol Drwawnbale {
    init(x: Int, y: Int)
 }

class Person: Drwawnbale {
    required init(x: Int, y: Int) {
        
    }
}

final class Test : Drwawnbale{
    init(x: Int, y: Int) {
        
    }
}

```

协议初始化定义，与父类初始化相同 实现必须加required override
```
protocol Drwawnbale {
    init(x: Int, y: Int)
 }

class Person {
    init(x: Int, y: Int) {
    }
}

class Student: Person,Drwawnbale{
    required override init(x: Int, y: Int) {
        super.init(x: x, y: y)
    }
}

```

### is as as? as!
is来判断是否是某种类型， as来做强制转换

```
protocol Runable{
    func run()
}

class Person{}
class Student: Person, Runable {
    func run() {
        
    }
    func study() {
    }
}

var stu: Any = 10
print(stu is Int)
stu = Student()
print(stu is Student)
```

as? as!

```
protocol Runable{
    func run()
}

class Person{}
class Student: Person, Runable {
    func run() {
        
    }
    func study() {
        print("study")
    }
}

var stu: Any = 10

(stu as? Student)?.study()
stu = Student()

(stu as! Student).study()//study

(stu as? Student)?.study()//study
```
