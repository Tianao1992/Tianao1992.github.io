---
title: 闭包
date: 2021-02-03 13:45:19
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 闭包
**-个函数和它捕获的变量或常量组合起来，成为闭包**

- 一般是定义在函数内部的函数

- 一般它捕获的是外层函数的局部变量或常量

```
typealias Fn = (Int) -> Int

func getFn () -> Fn {
    //申请一块堆空间内存 保存num
    var num = 0
    func plus (_ i: Int) -> Int {
        num += i
        return num
    }

    num = 14
    return plus // 捕获发生在返回
}
```
plus和num形成了闭包

**可以把闭包想成一个类的实例对象，类似于oc里block**
- 内存在堆空间
- 捕获的变量或常量是实例的成员变量
- 组成闭包的函数是类内部定义方法

上述示例代码等价于
```
class clouser {
    var num = 0
    func plus(_ i : Int) -> Int {
        num += i
        return num
    }
}
```

#### 闭包的循环引用
与 OC 中的 block 类似，Swift 闭包也会强引用被它捕获的对象，从而引发可能的循环引用问题。
本身调用了对象的属性或方法，从而捕获了本身，造成循环引用问题

```
class Person {
    var fn: (() -> ())?
    func run(){
        print("run")
    }
    deinit {
        print("deinit")
    }
}
func test(){
    let p = Person()
    p.fn = {
        p.run()
    }
}
```
解决循环一样的两种办法

- weak 实例销毁后，ARC会将弱引用自动置为nil，不会触发属性观察器

- unowned 实例销毁后，任然存储这实例对象的内存地址，试图访问会引起崩溃
```
 p.fn = {
        [weak p] in
        p?.run()
}

  p.fn = {
        [unowned p] in
        p.run()
    }
```

#### @escaping

- 非逃逸闭包，一般都当做函数参数使用,闭包调用发生在函数结束之前，在函数作用域内

- 逃逸闭包，有可能在函数调用之后执行，逃出函数作用域，需要用 @escaping进行修饰

```
typealias Fn = () -> ()

func test1(_ fn: Fn) {
    fn()
}

//逃逸闭包
var getFn:Fn?
func test2(_ fn: @escaping Fn) {
    getFn = fn
}
```
**逃逸闭包无法捕获inout参数**

```
typealias Fn = () ->()

func other(_ fn: Fn){fn()}
func other1(_ fn: @escaping Fn) { fn()}

func test(value: inout  Int) {
    other {
        value += 1
    }
    other1 {
        value += 1
    }
}
```
value什么时候用到在逃逸闭包中无法确定

### 闭包表达式
在swift中可以同func 定义一个函数，也可以通过闭包表达式定义一个函数

```
func sum(_ v1: Int ,_ v2: Int) -> Int {
    v1 + v2
}

var fn = {
    (_ v1: Int ,_ v2: Int) -> Int  in
    v1 + v2
}
```

#### 尾随闭包

如果将一个很长的闭包表达式作为函数的最后一个实参，使用尾随闭包可以增强函数的可读性 
尾随闭包是一个被书写在函数调用括号外面(后面)的闭包表达式
```
func test(_  v1: Int ,_ v2: Int, fn: (Int,Int) -> Int) {
    fn(v1,v2)
}
test(10, 20){
    $0 + $1
}
```

#### 自动闭包

使用autoclosure，值会被延迟执行

```
func getfirstpostin(_ v1: Int , _ v2: Int) -> Int{
    return v1 > 0 ? v1 : v2
}



func getfirstpostin(_ v1: Int, _ v2: @autoclosure () -> Int) -> Int? {
    return v1 > 0 ? v1 : v2()
}
```

