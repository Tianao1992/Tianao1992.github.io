---
title: iOS_UI视图汇总
date: 2020-12-10 10:47:27
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---
### 汇总：事件传递&响应链、图像显示原理、卡顿&掉帧、绘制原理&异步绘制

***

### UIView 和 CALayer 关系是怎么样的？
- UIView 为其提供内容，以及负责触摸等事件，参与响应链
- CALayer 负责显示内容contens
![](/img/ios/UI/uiview.png)

UIView和CAlayer从系统设计上符合 单一事件原则

***
### 系统UI事件传递及视图响应

#### 事件传递

当一个用户事件产生的时候，UIKit 会创建一个事件对象来描述这个用户事件。然后它会将该事件对象放进UIApplication对象所维护的事件队列中。对于触摸事件而言，产生的事件对象便是一个包含UIEvent的集合(NSSet)对象，一个事件会朝着特定的路径进行传递直到遇到一个可以处理它的对象为止

***事件传递的两个重要方法***

- -(UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event

- -（BOOL）pointInside:(CGPoint)point withEvent:(UIEvent *)event

***事件传递流程图***
![](/img/ios/UI/hitTest.png)

倒序遍历：最后添加的UIView最优先被遍历
hitTest如果有值，返回当前视图，结束事件传递,如果没有UIWindow就作为一个响应的视图

***hitTest:WithEvent系统是实现如图***
![](/img/ios/UI/hitTestEvent.png)

1. 优先判读当前视图的：hidden alpha  等属性，如果不满足返回nil,也就是说当前视图，不作为响应者，然后在由父视图遍历同级兄弟视图

2. 判读点击的点是否在当前视图，如果不满足返回nil,也就是
说当前视图，不作为响应者，然后在由父视图遍历同级兄弟视图

3. 以倒序方式遍历当前视图的所有子视图，遍历过程 调用 hittest 方法,如果有对应的响应视图就返回，如果遍历完成之后，没有对应视图，由于 pointinside 在当前视图范围内，返回当前图作为响应视图返回给调用方

#### 响应链

响应者链条是由响应者对象构成的，它们有一个共同点就是都是继承自UIResponder，UIResponder的程序接口不仅定义了事件处理方式，还有响应者的一些共同行为。UIApplication，UIViewController和UIView都是响应者，这意味着UIView与其所有子类和大多数主要控制器对象都是响应者，符合设计模式中责任链

![](/img/ios/UI/responder.png)

 - 响应链:由离 户最近的view向系统传递。initial view –> super view –> .....–> view controller –> window –> Application –> AppDelegate

 - Hit-Testing 链:由系统向离户最近的view传递。 UIKit –> active app's event queue –> window –> root view –>......–>lowest view 

***

### 图层显示原理


***cpu和gpu都做了哪些事情***

![](/img/ios/UI/imagePrinciple.png)

cpu 和 gpu 两个是通过总线连接起来，那么我们在cpu中所输出结果往往是一个位图，再经由总线在合适的时机上传给 gpu, gpu 在拿到位图之后，做相应位图图层的渲染包括纹理合成之后再把结果放到帧缓冲区域当中，由视频控制器根据vsync信号在指定时间之前，去提取对应帧缓冲区域当中屏幕显示内容，然后最终显示在手机屏幕上

***UI 视图显示在屏幕上的大致过程***

![](/img/ios/UI/iamgePrinciple1.png)

首先当我们创建了一个UIview 显示控件后，它的显示部分是由CAlayer 来负责，CAlayer 当中有一个contents 属性，就是我们最终要绘制到屏幕上面的位图，比如说我们创建了一个UIlabel,那么contens里面最终放的结果就是关于hello world的一个文字的位图，然后系统会在合适的时机，回调给我们一个drawRect方法，然后我们可以在此基础之上绘制一些，我们想自定义绘制的内容，绘制好的这个位图，最终经由Core Animation这个框架提交给 gpu 部分的openGL 渲染管线，进行最终位图的渲染和纹理合成，然后会显示我们的屏幕上。

***cpu工作***
![](/img/ios/UI/cpu.png)

- layout: UI布局 文本的计算  
- display: 绘制  
- prepare: 图片编解码 
- commit: 提交位图

***gpu工作***

![](/img/ios/UI/gpu.png)

### UI卡顿、掉帧的原因

***如图***

![](/img/ios/UI/uikadun.png)

- 一般说页面滑动的流畅性是60fps , 指的就是每一秒中会有60帧的画面更新，我们在人眼上面看到的就是流畅效果，基于此每隔16.7ms就要产生一帧画面，在这16.7ms内需要 cpu 和 gpu协同完成这一帧的数据.
- 比如 cpu 花费一定的时间做 UI布局 文本计算 视图绘制 图片编解码 然后把产生位图提交给 gpu 进行渲染和纹理合成。准好下一帧画面，然后在下一帧的vsync 信号到来时候就可以显示这么样的一个画面。假如说 cpu 占用时间过长，那么留给gpu的时间就非常少，gpu要想完成工作，那么总时间就会超过16.7ms,这样在下一帧的vsync 信号到来时候，我们没有准备好当下的这一帧画面，那就由此产生了掉帧，看到的效果就是滑动的卡顿。


**总结一句话，在规定的16.7ms内在下一帧vsync信号到来之前，并没有gpu和cpu共同完成下一帧画面的合成于是就会导致卡顿、掉帧**

***

### UIView 绘制原理

当UIview 调用 setNeedDisplay 系统会调用 calayer 的同名方法，之后相当与在当前 layer打上了一个脏标记，然后会在当前runloop将要结束的时候才会调用CALayer display方法，进入当前视图真正绘制流程。

![](/img/ios/UI/uiviwedisplay1.png)

#### 系统绘制流程

![](/img/ios/UI/calayerbacking.png)

#### 异步绘制流程
>- -[layer.delegate displayLayer]
    >- 代理负责生成对应的bitmap
    >- 设置该bitmap作为 layer.contents的属性的值
    、
![](/img/ios/UI/displayLayer.png)


### 离屏渲染

**离屏渲染：当我们指定UI视图的某些属性标记为在未愈合成之前不能用于当前屏幕显示就会触发离屏渲染，离屏渲染的概念起源于CPU层面，指的是GPU在当前屏幕缓冲区以新开辟一个缓冲区进行渲染操作**

#### 何时会触发？

- 圆角（当和 maskToBounds 一起使用时）
- 图层蒙版
- 阴影
- 光栅化

#### 为何要避免？

**在触发离屏渲染的时候，会增加GPU的工作量，而增加GPU的工作量很有可能会导致GPU和CPU的总耗时超过16.7ms,那么可能会导致UI掉帧和卡顿**

