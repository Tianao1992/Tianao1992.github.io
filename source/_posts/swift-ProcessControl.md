---
title: 流程控制
date: 2021-02-02 14:03:54
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### if else 

swift 中去掉了非0即真的概念 if 条件后面只能是bool类型
下面代码编译报错
```
let age = 1

if age { 
    print("123")
}
//Type 'Int' cannot be used as a boolean; test for '!= 0' instead

```

### while

```
var num = 5
while num > 0 {
    print(num) //打印五次
    num -= 1
}

var num = -1

repeat{
    print(num) //打印一次
} while num > 0

```

- repeat-while 相当于do-while

- swift去掉了 自减-- 和 自增++

### for 

#### 区间运算符

闭区间运算符 a...b   a<= 取值 <= b

```
let names = ["zhangsan","lisi","wangwu","zhaoliu"]
for i in 0...3 {
  print(names[i])
}

// for 默认是let 如果要使用加var 

```
```
for var i in 0...3 {
    i += 1
    print(i)// 1234
}
```

半开区间运算符   a..<b   a<= 取值 < b
```
let names = ["zhangsan","lisi","wangwu","zhaoliu"]
for i in 0..<3 {
  print(names[i])
}

```

单侧区间： 让区间方向尽可能的远
```
let names = ["zhangsan","lisi","wangwu","zhaoliu"]

for name in names[2...] {
    print("11",name)//11 wangwu  11 zhaoliu
}

for name in names[...2] {
    print("22",name)//22 zhangsan 22 lisi 22 wangwu
}

for name in names[..<2] {
    print("33",name) //33 zhangsan 33 lisi
}
```

### switch
去掉了 case default{},不写break 不会贯穿
```
var number = 1

switch number {
case 1:
    print("1")
case 2:
    print("2")
default:
    print("other")
}
```

**使用fallthrough可以实现贯穿**

```
var number = 1

switch number {
case 1:
    print("1")
    fallthrough
case 2:
    print("2")
default:
    print("other")
}

```
switch可以比配元祖
```
let num = (1,1)
switch num {
case (0,0):
    print("no")
case(-1,1):
    print("no")
case(-2,2):
    print("yes")
default:
    break
}
```

**可以值绑定，可以使用where条件过滤**
```
let point = (1,-1)
switch point {
case let (x,y)where x == y :
    print("no")
case let (x,y)where x == -y :
    print("yes")
default:
    break
}
```