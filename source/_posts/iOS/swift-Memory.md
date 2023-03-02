---
title: 内存管理
date: 2021-02-05 09:54:30
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### swift内存管理

跟oc一样，swift也是采取基于引用计数的ARC内存管理方案

swift的ARC有3中引用

- 强引用，默认情况下,引用都是强引用

- 弱引用（weak）,通过weak定义弱引用（必须是可选类型var,实例销毁后，ARC会自动将弱引用置为nil ,ARC给弱引用置为nil时，不会触发属性观察器）

- 无主引用（unowned）,通过无主引用unowned定义无主引用
（不会产生强引用，实例销毁后仍然存储着实例对象内存地址，在试图访问实例销毁后的无主引用，会产生运行时错误）

weak、unowned只能作用在类实例上
```
class Person {
    deinit {
        print("deinit")
    }
}
func test() {
   weak var p: Person? = Person()
    
}
```

### 循环引用
![](/img/ios/swift/class/xunhuan.png)

### 访问冲突

- 至少有一个写入操作

- 它们访问的是同一块内存

- 它们访问的时间重叠

```
var  a = 1
func test(_ num: inout Int) {
    num += a
}

test(&a)
//解决访问冲突

var  a = 1

func test(_ num: inout Int) {
    num += a
}

var b = a
test(&b)
```