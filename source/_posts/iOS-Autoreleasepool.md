---
title: iOS_Autoreleasepool
date: 2019-12-14 10:44:56
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### 什么是自动释放池

- 是以栈为结点通过双向链表形式组合而成

- 是和线程一一对应


ARC下，我们使用@autoreleasepool{}来使用一个AutoreleasePool，随后编译器将其改写成下面的样子:

![](/img/ios/autoreleasePool/autoreleasby.png)

**push转化**
![](/img/ios/autoreleasePool/push.png)

**pop转化**
![](/img/ios/autoreleasePool/pop.png)

一次pop实际上是相当于一次批量的pop操作


#### AutoreleasePoolPage

而这两个函数都是对AutoreleasePoolPage的简单封装，所以自动释放机制的核心就在于这个类

##### AutoreleasePoolPage数据结构

![](/img/ios/autoreleasePool/page.png)

- AutoreleasePool并没有单独的结构，惹事由若干个AutoreleasePoolPage以双向链表的形式组合而成

- AutoreleasePool是按线程一一对应的（结构中的thread指针指向当前线程）

- AutoreleasePoolPage每个对象会开辟4096字节内存（也就是虚拟内存一页的大小），除了上面的实例变量所占空间，剩下的空间全部用来储存autorelease对象的地址

- 一个AutoreleasePoolPage的空间被占满时，会新建一个AutoreleasePoolPage对象，连接链表，后来的autorelease对象在新的page加入

- 上面的id *next指针作为游标指向栈顶最新add进来的autorelease对象的下一个位置

##### AutoreleasePoolPage::push

![](/img/ios/autoreleasePool/pagepush.png)


每当进行一次objc_autoreleasePoolPush调用时，runtime向当前的AutoreleasePoolPage中add进一个哨兵对象，值为0（也就是个nil）

##### [objc autorelease] 流程

![](/img/ios/autoreleasePool/liuchen.png)


##### AutoreleasePoolPage::pop

- 根据传入的哨兵对象地址找到哨兵对象所处的page

- 在当前page中，将晚于哨兵对象插入的所有autorelease对象都发送一次- release消息，并向回移动next指针到正确位置

![](/img/ios/autoreleasePool/pagepop1.png)

![](/img/ios/autoreleasePool/pagepop2.png)


#### 多层嵌套的AutoreleasePool、

知道了上面的原理，嵌套的AutoreleasePool就非常简单了，pop的时候总会释放到上次push的位置为止，多层的pool就是多个哨兵对象而已，就像剥洋葱一样，每次一层，互不影响