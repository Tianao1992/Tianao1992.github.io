---
title: iOS 内存管理
date: 2020-12-14 09:18:12
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### 汇总：内存分布、内存管理方法、数据结构、ARC&MRC 、weak 

#### 内存分布

Objective-C是基于C的，不放看下C程序进程的内存分布

![](/img/ios/arm/buju.png)

可以分为部分

- .text: 代码区存放函数，程序结束后由系统释放

- .data: 已经初始化的全局变量和静态变量的一块内存区域。数据段属于静态内存分配,可以分为只读数据段和读写数据段。字符串常量等,是放在只读数据段中，结束程序时才会被收回。


- .bss: 存放程序中未初始化的全局变量和静态变量的一块内存区域

- heap:堆区存放进程运行中被动态分配的内存段,它的大小并不固定,可动态扩张或缩减。


- stack: 由编译器自动分配并释放，存放函数的参数值，局部变量等

#### 内存管理方案

- TaggedPointer （小对象管理 短string 、NSNumber、NSdata等）

- NONPOINTER_ISA （iOS arm64位架构下 非指针型isa）

- 散列表 （引用计数表、弱引用表）

##### 散列表方式

##### 散列表结构

![](/img/ios/arm/sideTables.png)

##### SideTable结构

![](/img/ios/arm/sidetable.png)

**思考：为什么不用一张表管理？**

假如说只有一张sideTable,我们在内存中所有分配的对象，它们引用计数或者弱引用存储都在这张大表当中，我们要操作它们引用计数值，有可能它们是在不同的线程中进行操作吗，由于数据安全访问问题，所以我们要进行加锁，那么这样就会存在一个效率问题。

![](/img/ios/arm/sidetablesuo.png)

##### 散列表数据结构

- Spinlock_t

- RefcountMap 

- weak_table_t

spinLock_t 特点

- 是 "忙等"的锁

- 适用于轻量访问


如果当前锁已被其他线程所获取，当前线程会不断的探测这锁有没有被释放，如果释放，自己第一时间去获取，所以说自旋锁是一种忙等的锁

再比如其他锁，如信号量，如果当前线程获取不到这个锁，它会把当前线程进行阻塞休眠，等到其他线程
释放这个锁的时候，再唤醒这个线程


##### 引用用计数

引用计数分为两种：

- 手动引用计数（MRC）

- 自动引用计数（ARC）

OC的内存机制可以简单概括为：谁持有(retain)谁释放(release)。retain引用计数+1，release反之

**ARC**

- ARC 是LLVM和Runtime协同的结果

- ACR 中禁止手动调用 retain/release/retainCount/dealloc

- ACR 新增weak、strong 属性关键字


##### 引用计数管理

实现原理分析

- alloc

- retain

- release

- dealloc

>- alloc实现
   经过一系列的调用，最终调用了C函数calloc
   此时并没有设置引用计数为1

>- retain 实现

![](/img/ios/arm/retain.png)


我们在进行retain 操作的时候，系统是怎样查找它对应引用计数的？是经过两次hash查找，查找到它对应的引用计数，然后再响应进行 +1 操作

>- release实现

![](/img/ios/arm/release.png)

>- dealloc 实现

![](/img/ios/arm/dealloc.png)

#### 弱引用管理（weak）

weak 关键字的作用弱引用，所引用对象的计数器不会加一，并在引用对象被释放的时候自动被设置为 nil

**weak编译**

![](/img/ios/arm/weakgl.png)


**weak变量怎样添加到弱引用表？**

![](/img/ios/arm/addweak.png)

一个被声明为 __weak的对象指针，经过编译器编译之后会调用 objc_initWeak 方法，经过一系列的函数调用栈，最终在weak_rgister_no_lock()这样一个函数中，进行弱引用变量添加，具体添加的位置是通过哈希算法进行位置，如果查找到对应位置已经有了所对应的弱引用数组，就把新的弱引用用变量添加到数组中，如果没有就创建一个新的弱引用数，第0位置添加新的弱引用指针，其他位置初始化为nil

**weak释放为nil过程**

![](/img/ios/arm/deallocweak.png)
当一个对象被dealloc,在dealloc内部实现当中会调用弱引用相关清理函数，在这个函数内部实现当中会根据当前对象的指针，查找弱引用表，把当前对象相应的弱引用都拿出来，是一个数组，遍历数组当中所有弱引用指针，分别置为nil

