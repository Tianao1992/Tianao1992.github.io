---
title: 枚举
date: 2021-02-02 15:26:36
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 枚举基本用法

```
enum Direction {
    case north,south,east,west
}

var dir = Direction.west
dir = .north

switch dir {
case .north:
    print("north")
case .east:
    print("east")
case .south:
    print("south")
case .west:
    print("west")
}

```

### 关联值

将枚举成员值跟其他类型值关联储存在一起
```
enum Password {
   case number(Int ,Int,Int, Int)
   case getsture(String)
}
var pwd = Password.number(1, 3, 4, 5)
pwd = .getsture("1345")

switch pwd {
case let .number(a1 , a2, a3, a4):
    print(a1,a2,a3,a4)
case let .getsture(str):
    print(str)
}

```

### 原始值，隐式原始值

- 枚举成员可以使用相同类型的默认值预先对应这个默认值，叫做原始值

- 原始值不占用枚举变量内存

- 如果枚举的原始值类型是 Int,String会自动分配原始值

```
enum Direction: String {
    case north,south,east,west
}

print(Direction.north.rawValue)//"north"
print(Direction.west.rawValue)//"west"



enum Direction: Int {
    case north,south,east,west
}
print(Direction.north.rawValue)// 0
print(Direction.west.rawValue)// 3
```

### 递归枚举

```
indirect enum Airpext {
    case number(Int)
    case sum(Airpext,Airpext)
    case diference(Airpext,Airpext)
}


let five = Airpext.number(5)
let four = Airpext.number(4)
let sum = Airpext.sum(five, four)
```

### 关联值内存

```
enum Password {
   case number(Int ,Int,Int, Int)
   case other
}

MemoryLayout<Password>.stride // 40 分配占用空间大小
MemoryLayout<Password>.size// 33/实际占用
MemoryLayout<Password>.alignment// 8//对齐

```
