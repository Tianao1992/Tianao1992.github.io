---
title: 访问控制
date: 2021-02-05 09:06:05
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 访问权限

swift提供了5个不同的访问权限

- open: 公开权限, 最高的权限, 可以被其他模块访问, 继承及复写

- public: 允许在定义实体的模块，其他模块访问，不允许被其他模块继承重写

- internal: 只允许在定义的实体模块中访问，不允许在其他模块访问

- fileprivate: 只允许在定义的实体源文件中访问

- private: 只允许在定义实体的封闭空间访问

### 访问级别规则

- 一个实体不可以被更低访问级别的实体定义，如下：

1. 变量\常量类型  ≥ 变量\常量
2. 参数类型、返回值类型  ≥ 函数
3. 父类 ≥子类
4. 父协议 ≥子协议
5. 原类型 ≥typealias
6. 原始值类型、关联值类型 ≥枚举类型
7. 定义类型A时用到的其他类型 ≥类型A

 ......

### 元祖、泛型
元祖访问级别是成员中访问级别最低的那个
```
internal class Dog {}
fileprivate class Animal {}

fileprivate var data: (Dog, Animal) // (Dog, Animal)的访问级别是fileprivate
```
泛型类型的访问级别是类型访问级别以及所有泛型类型参数参数访问级别最低那个

```
internal class Dog {}
fileprivate class Cat {}
public class Animal<T1, T2> {}

fileprivate var animal = Animal<Dog, Cat>() // Animal<Dog, Cat>()的访问级别是fileprivate
```
### 成员、嵌套类型

类型的访问级别会影响成员（属性、方法、初始化器、下标）、嵌套类型的访问级别

- 一般情况下，类型为private 或 fileprivate，那么成员\嵌套类型默认也是private 或 fileprivate


- 一般情况下，类型为internal 或 public，那么成员\嵌套类型默认是internal

```
private class Test {
    var name: String = "zhansan" //private
}

```

### getter、setter

- getter、setter默认自动接收它们所属环境的访问级别

- 可以给setter单独设置一个比getter更低的访问级别，用以限制写的权限
注意：getter的权限不能比setter低

```
num可以在其他实体中被访问，但是只能在当前文件中被修改
fileprivate(set) public var num = 5

class Person {
    private(set) var name = "lisi"
    fileprivate(set) public var age: Int {
        set {}
        get { 10 }
    }
}
```

### 初始化器

- 如果一个public类想在另一个模块调用编译生成的默认无参初始化器，必须 public的无参初始化器

-  因为public类的默认初始化器是internal级别（在写一些提供给别人用的库的时候会用到）

- required初始化器≥它的默认访问级别

- 如果结构体有private\fileprivate的存储实例属性，那么它的也是private\fileprivate

### 枚举类型的case、协议

- 不能给enum的每个case单独设置访问级别

- 每个case自动接收enum的访问级别

自动接收协议的访问级别，不能单独设置访问级别

协议实现的访问级别必须 ≥ 类型访问级别,或者 ≥ 协议访问级别

```
public protocol TestProtocol {
    func run()
}

public class Person : TestProtocol {
    public func run() {
        print("run")
    }
}
```

### 扩展

- 如果有显示设置扩展的访问级别，扩展添加的成员自动接收扩展的访问级别

- 如果没有显示设置扩展的访问级别，扩展添加的成员默认访问级别，跟直接在类型中定义的成员一样

- 可以单独给扩展中添加的成员设置访问级别

- 不能给遵守协议的扩展显示设置扩展的访问级别


在同一个文件中的扩展，可以写成类似多个部分的类型声明
- 在原本的声明中，声明一个私有成员，可以在同一个文件的扩展中访问它。
- 在扩展中声明一个私有成员，也可以在同一个文件的其他扩展中，或者原本的声明中访问它。

```
public class Person {
   private func run() {
        run1()
    }
}

extension Person {
    private func run1() {
         
     }
}
p
```

