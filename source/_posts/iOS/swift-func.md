---
title: 函数
date: 2020-12-02 14:34:55
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 函数的定义
形参是let类型， 也只是是let
```
func pi() -> Double {
    return 3.14
}

func sum(v1: Int ,v2: Int) -> Int {
     v1 + v2
}
sum(v1: 10, v2: 20) // 30

//无返回值 无参
func sayHello(){
    print("hello")
}

```

### 返回元祖，实现多返回值
```
func calculate(v1: Int, v2: Int) -> (sum: Int,differnce: Int,average: Int) {
    let sum = v1 + v2
    return(sum,v1 - v2, sum >> 1)
}

let result = calculate(v1: 10, v2: 20)
result.sum //30
result.differnce// -10
result.average// 15
```

### 参数标签
下划线省略参数标签
```
func sum(_ v1: Int, _ v2: Int) {
    v1 + v2
}
```

### 参数默认值

调用函数可不传默认值标签
```
func test(name: String = "zhangsan" ,age: Int) {
    print(name,age)
}
test(age: 10)
```

### 可变参数

- 一个函数最多只能有一个可变参数

- 紧跟在可变参数后面的参数不能使用省略标签

```
func test(_ numbers: Int... ,name: String ,_ age: Int) {
    print(name,age)
}
test(20,30,20, name:"zhangsan",40)
```

### 输入输出参数（IN-OUT）

- 可以使用inout 定义输入输出参数，可以在函数内部修改外部实参值
- 可变参数不能使用 inout
- inout 参数不能有默认值
- inout 参数只能传入可以被多次赋值的
- inout 本质是引用传递

```
func swapvalue(_ v1: inout Int ,_ v2: inout Int) {
    let temp = v1
    v1 = v2
    v2 = temp
}
var a1 = 10
var a2 = 20
swapvalue(&a1, &a2)
```

### 函数重载

- 函数名相同

- 参数标签不用，个数不同，类型不同

```
func test(v1:Int,v2: Int) -> Int {
    v1 + v2
}

func test(a1: Int, a2: Int, a3: Int) -> Int {
    a1 + a2 + a3
}

func test(v1: Int , v2: Double) -> Double {
    Double(v1) + v2
}

```

### 内联函数
如果开启编译器优化，编译器会将某些函数变成内联函数
那些函数不会被内联？

- 函数体比较长

- 包含递归调用

- 包含动态派发


永远不会内联
```
@inline(never ) func test() {
    print("test")
}
```
函数体很长也会内联
```
@inline(__always ) func test() {
    print("test")
}
```

### typealias
```
typealias IntFN = (Int,Int) -> Int

func test () -> IntFN {
    sum
}
func sum (_ a: Int, _ b: Int) -> Int {
    a + b
}
let fn =  test()
fn(10,20)//30
```

### 函数嵌套
```
func fowrard(_ forward: Bool)  -> (Int) -> Int {
    func next (_ input: Int) -> Int {
      input + 1
    }
    func previous (_ input: Int) -> Int {
        input - 1
    }
    
    return forward ? next : previous
}
fowrard(true)(3) //4
fowrard(false)(3)//2
```