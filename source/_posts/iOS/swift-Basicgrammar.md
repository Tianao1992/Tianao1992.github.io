---
title: 基础语法
date: 2020-12-02 13:34:20
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 常量

- 只能赋值一次

- 它的值不要求在编译时期决定，但在使用之前只能赋值一次

- 常量、变量在未初始化之前都不能使用

```
let age = 10

let age2: Int = 20
age2 = 2 //Cannot assign to value: 'age2' is a 'let' constant

```

### 标识符

- 标识符（常量名、变量名、函数名）几乎可以使用任何字符

- 标识符不能已数字开头，不能已空白字符、制表符、箭头字符等特殊字符

```
let 🐂 = 10
let 👽 = 20
```
### 字面量
```
let bool = true

let str = "test"

let char: Character = "s"

let int = 10

let double = 10.0

let arr = [1,3,4,5]

let dic = ["age":19, "height":20]
```

### 元祖

元组中的值可以是任何类型，并且不需要是相同类型
```
let tuple = (20,"test")

let (age,str) = tuple
```

### 常见数据类型

**swift大致可以分为两种，一种是值类型，一种是引用类型**
如图

![](/img/ios/swift/basic/type.png)

