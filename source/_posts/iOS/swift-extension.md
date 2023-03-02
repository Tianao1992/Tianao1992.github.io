---
title: 扩展
date: 2021-02-04 16:21:06
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---
### 扩展
swift中扩展类似于oc 中分类
**扩展可以为枚举、类、结构体、协议增添新功能**
可以添加，方法，计算属性、下标、便捷初始化器、协议等

**扩展不能做的事**

- 不能覆盖原有功能

- 不能添加存储属性，不能向已有的属性添加属性观察器

- 不能添加父类

- 不能添加指定初始化器与反初始化器

```
extension Array {
    subscript(nullable idx: Int) -> Element? {
        if (startIndex..<endIndex).contains(idx) {
            return self[idx]
        }
        return nil
    }
    
}

var arr = [1,4,3,2]

var a = arr[nullable: 2]! 
print(a)
```

### 协议
扩展可以给协议提供默认实现，也可以间接提供可选协议
```
protocol TestProtocol {
    func test()
}

extension TestProtocol {
    func test() {
        print("test")
    }
    func test1() {
        print("test1")
    }
}
```
如果一个类已经实现了协议所有要求，但还没有声明遵守它，可以通过扩展来实现
```
protocol TestProtocol {
    func test()
}

class testClass {
    func test() {
        print("test")
    }
}
extension testClass: TestProtocol{}
```

### 泛型
```
class Stack<E> {
    var elements = [E]()
    func push(_ element: E) {
        elements.append(element)
    }
}
extension Stack {
    func  top() ->  E{
        elements.last!
    }
}

```



