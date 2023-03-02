---
title: ios_Runtime
date: 2019-10-11 16:35:03
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### runtime 介绍

Objective-C 扩展了 C 语言，并加入了面向对象特性和 Smalltalk 式的消息传递机制。而这个扩展的核心是一个用 C 和 编译语言 写的 Runtime 库。它是 Objective-C 面向对象和动态机制的基石

#### runtime 基础数据结构

![](/img/ios/runtime/runtimejc.png)

- objc_object

- objc_class

- isa 指针

- method_t

#### objc_object 数据结构


![](/img/ios/runtime/objcobject.png)


#### objc_class 数据结构

![](/img/ios/runtime/objcclass.png)

bits: 一个类的变量、属性、方法 在这个数据结构当中

#### isa 指针
 c语言中 共用体isa_t

 - 指针型isa 代表class的地址

 - 非指针型isa 部分代表class的地址

![](/img/ios/runtime/isa.png)

**isa指向**

![](/img/ios/runtime/isazx.png)

- 如果我们调用实例方法 isa 去类对象中进行查找

- 如果我们调用的是类方法 isa 去元类对象中查找


#### cache_t 作用

- 用于快速查找方法执行函数

- 是可增量扩展的哈希表结构

- 是局部原理的最佳应用


#### cache_t 大致结构

![](/img/ios/runtime/cachet.png)

> -流程：根据key通过哈希查找算法，来定位这个key所对应的这个bucke_t 这个数据结构位于数组那个位置，当我们定位到这个位置之后，就可以通过提取bucket_t 里面具体函数实现来调用这个函数
key: 就是对应SEL 
imp: 无类型的函数指针

#### class_data_bits_t 介绍

- class_data_bits_t 主要是对class_rw_t的封装

- class_rw_t 代表类的相关读写信息，对class_ro_t的封装

- class_ro_t 代表了类相关的只读信息

**class_rw_t**

![](/img/ios/runtime/classrw.png)


- 在我们为一个类添加的分类里的一些方法、协议、属性都在class_rw_t 数据结构当中

**class_ro_t**

![](/img/ios/runtime/classro.png)


**method_t**

![](/img/ios/runtime/methodt.png)


**Type Encodings**

![](/img/ios/runtime/typs.png)

#### 对象、类对象、元类对象

- 类对象存储实例方法列表等信息

- 元类对象存储类方法列表等信息


**消息传递指向isa superclass图**

![](/img/ios/runtime/msgcd.png)
- 元类对象的isa 都指向根元类对象

- 根元类对象 superclass 指向根类对象，当我们调用类方法时，如果在元类对象中逐级父类查找，如果找不到，它就会找根类对象中同名的实例方法


#### 消息传递

**两个方法**

>- void objc_msgSend(void/*id self, SEL op,...*/)

>- [self class] -> objc_msgSend(self,@selectior(class))


>- void objc_msgSendSuper(void/* struct objc_super *super ,SEL op, ... */)

>- [super class] -> objc_msgSendSuper(super,@selector(class))

super 实际上是编译器关键字，recevier 接收者就是当前对象


**消息传递流程**

![](/img/ios/runtime/msgz.png)

**缓存查找**

![](/img/ios/runtime/cachecz.png)

- 通过给定的key,根据哈希查找，找出bucket_t 在数组对应的位置，然后获取 imp

**当前类中查找**

- 对于已排序好的列表，采用二分查找算法查找方法对应的执行函数
- 对于没有排序的列表，采用一般遍历查找方法对应的执行函数


**父级逐级查找**

![](/img/ios/runtime/superclass.png)


#### 消息转发流程

![](/img/ios/runtime/msgzf.png)

#### @dynamic 关键字

- 动态运行时语言将函数决议推迟到运行时

- 编译时语言在编译器进行函数决议

**能否向编译后的类添加增加实例变量？**

不能 编译之前定义创建类已经完成了实例变量的布局，在具体分析runtime 数据结构 class_ro_t 是没有办法修改