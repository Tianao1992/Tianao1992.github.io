---
title: oc语言特性
date: 2020-12-11 11:19:41
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### 汇总：分类 、关联对象、扩展、通知、kvo、kvc、属性关键字

***

### 分类

#### 分类都做了哪些事情
- 声明私有方法
- 分解体积庞大的类文件
- 把Framework的私有方法公开

#### 分类的特点
- 运行时决议
- 可以为系统类添加分类

#### 分类都可以添加哪些内容

- 实例方法
- 类方法
- 协议
- 属性 （只是声明了get 和 set 方法 ，并没有添加实例变量）

**分类的数据结构**

![](/img/ios/octx/categor.png)

**分类调用栈**

![](/img/ios/octx/categorystaic.png)

image: 表示的是镜像
read_image:读取镜像，加载一些可执行文件到内存中进行处理

**添加的两个分类a和b 有同名方法那个生效？**

源码分析

![](/img/ios/octx/categorycode.png)


>- 最先访问，最后编译的分类，添加的两个分类a和b 有同名方法那个生效，取决于分类的编译顺序，最后编译的那个分类才会生效，前面的会被覆盖掉

**分类覆盖宿主类**

![](/img/ios/octx/categorycode1.png)

![](/img/ios/octx/categorycode2.png)

- 分类添加的方法“覆盖”原类方法
- 同名分类方法谁能生效取决于编译顺序
- 名字相同的分类会引起编译报错

***

### 关联对象

#### 关联对象方法

//关联对象
1. void objc_setAssociatedObject(id object, const void *key, id value, objc_AssociationPolicy policy)

//获取关联的对象

2. id objc_getAssociatedObject(id object, const void *key)

//移除关联的对象

3. void objc_removeAssociatedObjects(id object)

#### 关联对象本质
- 关联对象由AssoctionsManager管理并在AssoctionsHashMap存储

- 所有对象的关联内容都在同一个全局容器中

![](/img/ios/octx/association.png)

***

### 扩展

#### 扩展做什么

- 声明私有属性

- 声明私有方法

- 声明私有成员变量

#### 扩展特性

- 编译时决议

- 只以声明的形式存在，多数情况下寄生于宿主类.m 中

- 不能为系统类添加扩展


### 通知（NSNOtification）

#### 定义

- 是使用观察者模式来实现的用于跨层传递消息的机制

- 传递方式为 一对多

#### 通知机制

![](/img/ios/octx/nsnotification.png)

### KVO

#### 定义

- kvo 是 key-value observing 的缩写

- kvo 是 oc 对观察者模式的又一种实现

- apple 使用了 isa 混写（isa-swizzing）来实现 kvo

![](/img/ios/octx/kvoyuanli.png)

isa_swizzing: 
          当我们调用 addObserver:forKeyPath:options:context 之后 系统会在运行时创建NSKVONotifying_* 这样一个类 ，同时将原来的类的isa 指针指向新创建的类。
           新创的类实际上是原来类的子类，之所以这做，是重写原来类的setter方法，来达到通知所有观察者对象

**重写的Setter添加的方法**

- -（void）willChangeValueForKey:(NSString *) key

- -（void）didChangeValueForKey:(NSString *) key

**kvo改变值生效问题**

- 使用setter方法改变值才会生效

- 使用setvalue:forkey 改变值会生效

- 成员变量直接修改需手动添加kvo才会生效

### KVC

#### 键值编码技术，会破坏面向对象思想


**两个方法**

- -（id)valueForKey:(NSString *)key

- - (void)setValue:(id)value forKey:(NNString *)key

**valueForKey 流程**

![](/img/ios/octx/valuekey.png)


**setValue 流程**

![](/img/ios/octx/setvaluekey.png)

***

### 属性关键字

#### 原子性

- atomic
- noatomic

atomic: 赋值获取保证线程安全，操作不保证

#### assign

- 修饰基本数据类型，如 int bool 等

- 修饰对象类时，不改变其引用技数

- 会产生悬垂指针

#### weak
- 不改变被修饰对象的引用计数
- 所指的对象在被释放后悔自动置为nil

#### copy

浅拷贝

![](/img/ios/octx/qicopy.png)

深拷贝

![](/img/ios/octx/mutablecopy.png)

cop总结

![](/img/ios/octx/copyzj.png)

- 可变对象的copy和mutableCopy都是深拷贝

- 不可变对象的copy是浅拷贝， mutableCopy是深拷贝

- copy方法返回的都是不可变对象
