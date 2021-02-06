---
title: 属性
date: 2021-02-03 10:29:32
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 属性介绍
swift跟实例相关的属性大致可以分为两种

**存储属性**

- 类似于成员变量的概念

- 可以存储在实例对象内存中

- 结构体、类可以定义存储属性 枚举不可以

**计算属性**

- 本质就是方法

- 不占用实例内存

- 结构体、类、枚举都可以定义计算属性


#### 存储属性

- 可以在初始化器里为存储属性设置一个初始化值

- 可以分配一个默认的属性值作为属性定义的一部分

#### 计算属性

set 传入的新值默认交newValue,也可以自定义

定义计算属性只能用var,因为计算属性随时都有可能发生变化

只读计算属性只有 get,没有set
```
struct Circle {
    var radius: Double
    var dirameter: Double {
        set (newDirameter){
            radius = newDirameter / 2
        }
        
        get {
            return radius * 2
        }
    }
}

```

**枚举rawValue本质: 只读计算属性**
```
enum TestEunm : Int {
    case test1 = 1, test2 = 2, test3 = 3
    
    var rawValue: Int {
        switch self {
        case .test1:
            return 1
        case .test2:
            return 2
        case .test3:
            return 12
        }
    }
}


print(TestEunm.test3.rawValue)//12
```


### 延迟存储属性

可以使lazy定义一个存储属性，当第一次使用到时才会进行初始化
- lazy属性必须是var,不能是le(let 必须在实例初始化化完成之前就拥有值)

- 如果多条线程同时第一次访问lazy属性 (无法保证初始化一次)

```
class Car {
    init() {
        print("car init")
    }
    func run(){
        print("car runing")
    }
}

class Person {
    //延迟属性
    lazy var car = Car()
    init() {
        print("person init")
    }
    func goout() {
        car.run()
        print("car go out")
    }
}

```

### 属性观察器
可以给为非lazy 存储属性设置属性观察器

- willSet会传递新值，默认叫newvalue

- oldValue 传递旧值，默认交oldvalue

- 在初始化器中设置初始值不会触发属性观察器

```
struct Circle {
    var radius: Double {
        willSet {
            print(newValue)
        }
        didSet {
            print(oldValue)
        }
    }
    init() {
        self.radius = 10
        print("Circle init")
    }
}
```

### 类型属性

只能通过类访问,可以通过static定义类型属性,如果是类可以通过class关键字定义,大致可以分为两种

- 存储型类型属性: 整个程序运行过程中只有一份内存

- 计算型类型属性

```
struct Preson {
    static var name: String = "zhangs"
    init() {
        Preson.name = "lisi"
    }
}
```

存储类型属性注意细节

- 不同与存储属性，类型属性必须设定初始值

- 存储类型属性默认是lazy,会在第一次调用时初始化

- 就算被多个线程同时访问，保证也是初始化一次


swift中单利写法

```
class FileMager {
    public static let share = FileMager()
    public init(){}
    
}
```
