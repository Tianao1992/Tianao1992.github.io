---
title: 泛型
date: 2021-02-04 15:03:25
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

泛型可以将类型参数化，提高代码复用率，减少代码量

```
func swapValus<T>(_ v1: inout T , _ v2: inout T)  {
    (v1,v2) = (v2,v1)
}
var a = 10
var b = 10
swapValus(&a, &b)
var c = 10.0
var d = 20.0
swapValus(&c, &d)
```

### 关联类型
给协议用的类型定义一个占位符，协议中可以定义多个关联类型
```
protocol Stackable {
    associatedtype Element
    mutating func push(_ element: Element)
    mutating func pop() -> Element
    func top() -> Element
    func size() -> Int 
}
class  Stack <E>: Stackable{
    var elements = [E]()
    func push(_ element: E) {
        elements.append(element)
    }
    func pop () -> E{
        elements.removeLast()
    }
    func top()  -> E{
        elements.last!
    }
    func size() -> Int {
        return elements.count
    }

}

```
