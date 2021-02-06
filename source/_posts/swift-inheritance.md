---
title: 继承
date: 2021-02-03 16:49:28
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---
### 继承介绍
1. 值类型（枚举、结构体）不支持继承，只有类支持继承 
2. 没有父类的类称为基类
3. 子类可以重写父类的属性、方法、下标,但必须加override**

#### 重写类型方法、下标

- 被class修饰的类型方法、下标可以被重写

- 被static修饰的类型方法、下标不可以被重写
```
class Animal {

    class func speak() {
        print("animal speak")
    }
    
    class subscript (index: Int)  -> Int{
        return index
    }
    class func run() {
        
    }
}

class Dog: Animal {
    override class func speak() {
        super.speak()
        print("dog speak")
    }
    
    override class subscript(index: Int) -> Int {
        return super[index] + 1
    }
}
```

#### 重写属性

- 子类可以将父类的属性（存储、计算）重写为计算属性

- 子类不可以将父类的属性，重写为存储属性

- 只能重写var属性

- 重写时，属性名、类型要相同

- 子类重写后的属性权限不能小于父类

#### 属性观察器

```
class Circle{
    var radus: Int = 1 {
        willSet{
            print("Circle willSet",newValue)
        }
        didSet {
            print("Circle didSet",oldValue,radus)
        }
    }
}
class  subCircle : Circle {
   override var radus: Int  {
        willSet{
            print("subCircle willSet",newValue)
        }
        didSet {
            print("subCircle didSet",oldValue,radus)
        }
    }
}

var circle = subCircle()
circle.radus = 10
/*
打印
subCircle willSet 10
Circle willSet 10
Circle didSet 1 10
subCircle didSet 1 10
*/
```

#### final
- 被final修饰的禁止重写 属性、下标、方法

- 被final修饰的类禁止继承


