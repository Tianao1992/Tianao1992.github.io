---
title: iOS  非指针型ISA(NONPOINTER_ISA)
date: 2020-12-09 15:56:46
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### 什么是NONPonter_isa？

 **在iOS arm64架构下，isa指针在内存中占64位/bit,但是实际上并不会使用所有的地址空间,为了提高内存的利用率，在剩余的bit**
 **存储了关于内存管理的一些相关内容，所以说这个叫非指针型的isa**

### 逐一分析这64个bit都存储那些内容

1. **0-15 位内存地址**

![](/img/ios/isa15.png)
- indexed:0 代表的是纯isa 指针代表类对象的内存地址 1代表这个isa存储的不仅是类对象的地址而且内存管理方面的数据
- has_assoc: 代表是否有关联对象  0 代表没有 1代表有
- hass_cxx_ditor: 当前对象是否有使用c++
- shifitcls: 当前对象类类对象的指针地址

2. **16-31 位内存地址**

![](/img/ios/isa31.png)

3. **32-47 位内存地址**

  ![](/img/ios/isa47.png)
- magic: 不影响内存管理方面解答
- wekkly_referenced: 标识了是否有 弱引用指针
- deallocating: 当前对象是否正在进行dealloc 操作
- has_sidetable_rc: 当前isa指针当中存储的引用计数已经达到了上限，需要外挂一个sidetable这样一个数据结构存储相关的引用计数内容
- extra_rc: 额外的引用计数 当引用计数很小的时候 存储在isa 指针当中

4. **48-63 位内存地址**

![](/img/ios/isa63.png)