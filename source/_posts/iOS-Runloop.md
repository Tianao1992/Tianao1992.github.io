---
title: Runloop
date: 2019-12-17 09:52:55
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### 什么是Runloop

从语义角度 runloop 是通过内部事件循环来处理事件/消息的一个对象

- 没有消息需要处理时，休眠避免占用资源 （用户态 到 核心态）

- 有消息处理时，立刻被唤醒 （核心态到用户态）

#### 为什么main 函数为什么能够保持一直 运行的状态而不退出？

在main函数中会启动主线程的runloop,而runloop又是对事件循环的一种维护机制，在有事做的可以去做事，没有事做的时候，从用户态到内核态进行切换，避免资源占用

![](/img/ios/runloop/main.png)

### runloop 数据结构

NSRunloop 是CFRunloop的封装，提供了面向对象的api

- CFRunloop

- CFRunloopMode

- source/Timer/observer

### CFRunloop 结构

![](/img/ios/runloop/cfrunloop.png)

从 pthread 可以看出是和线程一一对应

####  CFRunloopMode 分为哪些

-  UITrackingRunLoopMode (UI模式,只能被UI事件触发 优先级非常高)

- NSDefaultRunLoopMode (默认模式,处理时钟网络事件)

- NSRunLoopCommonModes (占位模式,以上两种模式都添加)

- UIInitializationRunLoopMode  App启动时第一个模式, runloop会处理启动事件,这个模式只运行一次(苹果不开放)

-  GSEventReceiveRunLoopMode系统事件的内部模式(苹果不开放)

####  CFRunloopSource 分为哪些

- source0 需要手动唤醒线程

- source1 具备唤醒线程的能力

####  CFRunloopObserver 观测事件点

- KCFRunLoopEntry  runloop 进入循环

- KCFRunLoopBeforeTimers  runloop 处理Timers事件前

- KCFRunLoopBeforeSources  runloop 处理 source 事件前

- KCFRunLoopBeforeWaiting  runloop 休眠前

- KCFRunLoopAfterWaiting  runloop 休眠后

- KCFRunLoopExit runloop 退出

####  各个数据结构之间的关系

runloop 对应mode 一对多 mode 对应 source、timer、observer 一对多

#### commmode的特殊性

- commmode不是实际存在的一种mode

- 是同步 source/Timer/observe 到多个mode 中的一种技术方案

### Runloop 机制

![](/img/ios/runloop/runloopjizhi.png)


### 怎样实现一个常驻线程

- 在当前线程中开启一个runloop 

- 向该runloop中添加一个Port/Source 等维持Runloop 的事件循环

- 启动该runloop


### 怎样保证子线程数据回来更新UI的时候不打断用户滑动操作?？
 - 用户滑动当前runloop 运行在UItrackingrunloopmode下

 -  我们一般在子线程中进行网络请求，所以可以在子线程给主线程数据进行UI更新的逻辑封装起来提交到主线       线程NSDefaultRunloopmode下 具体方法 [self performSelectorOnmainThred@selector(reloadData)       withobjc:nil waitUniteDone:No modes:NSDefaultRunloopmode]

 - 这样抛回来的任务，当用户滑动处于UITrackingRunLoopMode 下就不会执行，当滑动停止，当前mode切换NSDefaultRunloopmode再处理子线线程抛上来的任务