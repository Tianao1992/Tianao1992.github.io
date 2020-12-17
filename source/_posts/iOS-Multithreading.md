---
title: 多线程
date: 2020-12-16 15:46:48
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### iOS系统为我们提供那些多线程技术


主要是这三种

- GCD

- NSOperation

- NSTread


### GCD

#### 分为 同步/异步和 串行/并发

```
dispatch_sync(serial_queue,^(任务))
dispatch_async(serial_queue,^(任务))
dispatch_sync(concurrent_queue,^(任务))
dispatch_async(concurrent_queue,^(任务))

```

同步并发队列

下面代码输出啥

```
- （viewDidLoad）{
    NSLog(@"1");
    dispatch_sync(global_queue,^{
        NSLog(@"2");

        dispatch_sync(global_queue,^{

            NSLog(@"3");
         })；

        NSLog(@"4");
    })；
     NSLog(@"5");
}
```

同步阻塞当前线程 所以打印1之后执行 block 打印2 由于是并发队列操作 所以执行打印3，如果是串行造成死锁

 异步并发队列
下面代码输出啥
```
- （viewDidLoad）{
    NSLog(@"1");
    dispatch_async(global_queue,^{
        NSLog(@"2");
        [self performSelector：@selector(printlog) withObject:nil afterDelay:0]
        NSLog(@"3");
    })；
}
- (void)printlog{NSlog(@"2")}

```
输出13，block内线程没有 runloop,所以 performSelector不会执行

#### 怎样用GCd实现多读单写呢

dipatch_barrier_async()

![](/img/ios/multith/barrier.png)

### NSOperation
NSOperation 需要和 NSOperationqueue配合来实现多线程方案

特点

- 添加任务依赖

- 任务执行状态控制

- 最大并发量

####  任务执行状态

- isReady

- isExecuting

- isFinished

- isCancelled

状态控制

- 如果只是重写了main方法，底层控制变更执行完成状态，以及任务退出

- 如果重写start方法，自行控制任务状态

系统是怎样移除一个isfinished=Yes的NSOperation？

通过kvo

### NSTread

启动流程

![](/img/ios/multith/barrier.png)

一般我们用NSTread和runloop 结合来是一个常驻线程

### dispatch_semaphore_t

信号量：就是一种可用来控制访问资源的数量的标识，设定了一个信号量，在线程访问之前，加上信号量的处理，则可告知系统按照我们指定的信号量数量来执行多个线程。
异步线程下同步问题

//创建信号量，参数：信号量的初值，如果小于0则会返回NULL
dispatch_semaphore_create（信号量值）
 
//等待降低信号量
dispatch_semaphore_wait（信号量，等待时间）
 
//提高信号量
dispatch_semaphore_signal(信号量)



### 总结

- 一般我们用CGD来实现实现一些 简单线程同步 子线程放的分派，包括多读单写这些场景的一些问题的解决，
- 对于NSOpeation 由于它 任务依赖，任务状态控制，可以设置最大并发量的一些的特点，像afn sdwebimg 这些框架都会使用它

- 对于NSTread往往我们用它来实现一个常驻线程



