---
title: 初始化器
date: 2021-02-04 08:25:49
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 初始化器介绍
类、结构体、枚举都可以定义初始化器

类的初始化器有两种: 指定初始化器、便捷初始化器

- 每个类至少有一个指定初始化器，指定初始化器是类主要的初始器

- 默认初始化器是类的指定初始化器

- 类偏向于少量初始化器，一个类通常只要一个指定初始器

**初始化器互相调用规则**

- 指定初始化器必须从它的直系父类调用指定初始化器

- 便捷初始化器必须从它相同类里调用另一个初始化器

- 便捷初始化器最终必须调用一个指定初始化器

### 初始化器调用规则

![](/img/ios/swift/class/classchushihua.png)

### 两段式初始化

![](/img/ios/swift/class/lianduanchushi.png)


### 重写

- 当重写父类的指定初始化器，必须加上override

- 如果子类写了一个匹配父类便捷初始化，不用加override

### 自动继承

- 如果子类没有自定义任何初始化器，它会自动继承父类所有的指定初始化器

- 子类自动继承所有的父类初始化器


### required

用required修饰指定初始化器,表明所有子类都必须实现改初始器，如果子类重写required初始化器，必须加上required，不同加override

### 可失败初始器

![](/img/ios/swift/class/faileinit.png)

### 反初始化器

- deinit叫做反初始化器，类似于oc dealloc方法

- deinit不接收任何参数，不能包括小括号

- 父类的deinit能被子类继承

- 子类deinit执行完后调用父类deinit







