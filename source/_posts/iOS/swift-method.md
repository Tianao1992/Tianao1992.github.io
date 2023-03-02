---
title: 方法、下标
date: 2020-12-03 15:25:07
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 方法
类、枚举、结构体都可以定义实例方法、类方法

- 实例方法，通过实例对象调用

- 类方法 通过类型调用，通过static\class定义

```
class Car {
    //使用了 gcd once 保证线程安全
    
    //本质就是一个全局变量
    static var count: Int = 0
    
    static func getCounter () -> Int{
         self.count
    }
}
```
#### mutating

结构体和枚举值类型，默认情况下，自身属性不允许被自身实例方法修改
在func前头加  mutating可以修改
```
struct Point {
    var x = 0
    var y = 0
   mutating func move(deX: Int, deY: Int){
        x  += deX
        y  += deY
    }
}
```

#### @discardableResult

@discardableResult 可以消除返回值未使用的警告

```
@discardableResult
func getfn() ->Int {
    return 10
}
getfn()
```

### 下标
使用subscript可以给任意类型，增加下标功能
```
class Point {
    var x = 0.0, y = 0.0
    subscript (index: Int) -> Double {
        set {
            if index == 0 {
                x = newValue
            }else {
                y = newValue
            }
        }
        get {
            if index == 0 {
               return x
            }else {
               return y
            }
        }
        
    }
}
var p = Point()

p[0] = 11.0
p[1] = 12.0

print(p.x)
```

下标可以没有set方法，但必须有get方法，如果只有get 可以省略
```
class Point {
    var x = 0.0, y = 0.0
    subscript (index: Int) -> Double {
            if index == 0 {
               return x
            }else {
               return y
            }
        return 0
    }
}

```

下标有多个返回值

```
class Grid {
    var data = [[0,1,2,],[3,4,5],[6,7,8]]
    subscript(row: Int ,column: Int) -> Int{
        set{
            guard row >= 0 && row < 3 && column >= 0 && column < 3  else {
                return
            }
            data[row][column] = newValue
        }
        get {
            guard row >= 0 && row < 3 && column >= 0 && column < 3  else {
             return 0
            }

          return data[row][column]
        }
    }
}
var grid = Grid()
grid[0,1] = 77
grid[1,2] = 88
grid[2,0] = 99
print(grid.data)//[[0, 77, 2], [3, 4, 88], [99, 7, 8]]

```


