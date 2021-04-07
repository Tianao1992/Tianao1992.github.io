---
title: 可选项
date: 2020-12-02 16:02:45
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 可选项

- 可选项，一般也叫可选类型，它允许将值设为nil

- 在类型后面加？来定义可选项

```
var name: String? = "zhangsan"
name = nil

var age: Int? // 默认为nil
age = 20
```

### 强制解包

可选项是对其他类型一层包装，可以看做一个盒子

- 如果是nil,就是空盒子

- 非nil ,那么盒子里是被包装的类型数据

如果取出数据采用强制解包，值如果是nil的情况下，会产生运行是错误程序崩溃
```
var age: Int? // 默认为nil
age!
```

### 可选项绑定
**可以使用可选项绑定来判断可选项有没有值**

把值赋给一个临时变量或者常量，并返回true或false 

```
var age: Int? // 默认为nil

if let age1 = age {
    print(age1)
}else {
    print("值为nil 解包失败")
}
```

### where 循环中使用可选项绑定

```
//遇到非数字停止
var strs = ["20","9","test2","30","10"]

var index = 0
var sum = 0
while let num = Int(strs[index]), num > 0{
    sum += num
    index += 1
}
print(sum)// 29

```

### 空合并运算符

```
public func ?? <T>(optional: T?, defaultValue: @autoclosure () throws -> T) rethrows -> T
```

- a 是可选项

- b 是可选项或者不是

- a,b存储类型必须相同


1. 如果 a不为nil,返回a
2. 如果 a为nil ,返回b
3. 如果b不是选项，a自动解包

```
var a: Int? = 10
var b: Int = 20

a ?? b
```

### gurad语句

```
guard 条件 else {
   退出
}
```
- gurad 条件 为false 执行大括号内

- gurad 条件 为true 跳过gurad语句

- gurad 特别适合提前退出


**guard和if else对比**
```
func login (_ info:[String: String]) {
    let userName: String

    if let tmp = info["userName"] {
        userName = tmp
    }else {
        print("请输入用户名")
        return
    }

    let phone: String

    if let tmp1 = info["phone"] {
        phone = tmp1
    }else {
        print("请输入手机号")
        return
    }
    print("用户名\(userName)","手机号\(phone )")
}



func login (_ info:[String: String]) {
    guard let userName = info["userName"] else {
        print("请输入用户名")
        return
    }

    guard let phone = info["phone"] else {
        print("请输入手机号")
        return
    }
    print("用户名\(userName)","手机号\(phone )")
}
```
**gurad可选项绑定变量或常量也能在外层作用域使用**



