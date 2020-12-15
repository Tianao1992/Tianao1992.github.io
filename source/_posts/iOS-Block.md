---
title: iOS_Block
date: 2020-12-12 07:32:13
tags:
---


### Block 介绍

block 是将函数及其执行上下文分装起来的对象

例如
```
{
    int mutltiplier = 3;
    int(^Block)(int) = ^int(int num){
        return num * mutltiplier
    }
    Block(2)

}

``` 
经由编译编译之后，使用 clange -rewrite-objc  file.m 查看编译器内容
``` 
struct __block_impl {
    void *isa; isa指针，Block 是对象的标志
    int Flags;
    int Reserved;
    void *FuncPtr;// 无类型的函数指针
};

struct __main_block_impl_0 {
  struct __block_impl impl;
  struct __main_block_desc_0* Desc;
  __main_block_impl_0(void *fp, struct __main_block_desc_0 *desc, int flags=0) {
    impl.isa = &_NSConcreteStackBlock;
    impl.Flags = flags;
    impl.FuncPtr = fp;
    Desc = desc;
  }
};
static void __main_block_func_0(struct __main_block_impl_0 *__cself) {
}

static struct __main_block_desc_0 {
  size_t reserved;
  size_t Block_size;
} __main_block_desc_0_DATA = { 0, sizeof(struct __main_block_impl_0)};
int main(int argc, const char * argv[]) {
    /* @autoreleasepool */ { __AtAutoreleasePool __autoreleasepool;
        (void (*)())&__main_block_impl_0((void *)__main_block_func_0, &__main_block_desc_0_DATA);
    }
    return 0;
}
```

### block 截获变量特性

如下代码，看一下最终输出结果

```
{
    int mutltiplier = 3;
    int(^Block)(int) = ^int(int num){
        return num * mutltiplier
    }
    Block(2)
   mutltiplier = 4
   NSLog(@"result is %d",Block(2))
}
result is 6

``` 
使用 clange -rewrite-objc -fobjc-arc file.m

block 对以下类类型的变量都有哪些特点

- 局部变量 （基本数据类型 、 对象类型）

- 静态局部变量

- 全局变量

- 静态全局变量


对于基本数据类型的局部变量截获其值

对于对象类型的局部变量连同所有权修饰符一起截获

对于局部静态变量是以指针形式进行截获

对于全局变量、静态全局变量不截获

