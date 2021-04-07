---
title: iOS_Block
date: 2019-11-12 07:32:13
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
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


**block 对以下类类型的变量都有哪些特点**


- 局部变量 （基本数据类型 、 对象类型）

- 静态局部变量

- 全局变量

- 静态全局变量

``` 
int global_var = 4;

static int static_global_var = 5;

- (void)method {
    //基本数据类型
    int var = 3;
     //对象类型
    __unsafe_unretained id unsafe_obj = nil ;
    
    __strong id strong_obj = nil ;
    
    static int static_var = 7;
    
    void (^Block)(void) = ^{
        NSLog(@"局部变量<基本数据类型> var %d",var);
        NSLog(@"局部变量 <对象类型>var %@",unsafe_obj);
        NSLog(@"局部变量 <对象类型>var %@",strong_obj);
        NSLog(@"静态变量  %d",static_var);
        NSLog(@"全局变量 global_var %d",global_var);
        NSLog(@"静态全局变量 static_global_var %d",static_global_var);
    };
    Block();
    
    
}
``` 
使用 clange -rewrite-objc -fobjc-arc file.m

.cpp 编译文件中结果
``` 
int global_var = 4;

static int static_global_var = 5;


struct __McBlock__method_block_impl_0 {
  struct __block_impl impl;
  struct __McBlock__method_block_desc_0* Desc;
  int var;
  __unsafe_unretained id unsafe_obj;
  __strong id strong_obj;
  int *static_var;
  __McBlock__method_block_impl_0(void *fp, struct __McBlock__method_block_desc_0 *desc, int _var, __unsafe_unretained id _unsafe_obj, __strong id _strong_obj, int *_static_var, int flags=0) : var(_var), unsafe_obj(_unsafe_obj), strong_obj(_strong_obj), static_var(_static_var) {
    impl.isa = &_NSConcreteStackBlock;
    impl.Flags = flags;
    impl.FuncPtr = fp;
    Desc = desc;
  }
};

``` 

**总结**

- 对于基本数据类型的局部变量截获其值

- 对于对象类型的局部变量连同所有权修饰符一起截获

- 对于局部静态变量是以指针形式进行截获

- 对于全局变量、静态全局变量不截获


### __block 修饰符

 一般情况下，对截获变量值进行复制操作需要添加__blcok 修饰符

``` 
 {
    NSMutableArray *array = [NSMutableArray array];
    void(^Block)(void) = ^{
        [array addobjcet: @1234]
    }
    Block()
 }
```

是否使用__block 修饰符？

**__block 使用场景**

- 对于局部变量赋值需要

- 对于静态局部变量、全局变量、静态全局变量 不需要

静态局部变量是捕获指针、全局和静态全局不捕获值

**思考**
```
{
    __block int mutltiplier = 3;
    int(^Block)(int) = ^int(int num){
        return num * mutltiplier
    }
    Block(2)
   mutltiplier = 4
   NSLog(@"result is %d",Block(2))
}
result is 8

``` 
因为__blcok修饰的变量变成了对象

``` 
__block int multiplier  编译转化

struct _Block_byref_multiplier_0 {
void *_isa;
__Block_byref_multiplier_0 * _forwarding;
int _flags
int _size
int multiplier
}

(multiplier._forwarding -> multiplier )= 4

``` 
栈上的 __block forwarding指针指向自身



### block 内存管理

**block分为哪几类**

- NSConcreteGlobalBlock 全局block

- NSConcreteGStackBlock 栈block

- NSConcreteGMallocBlock 堆block


#### block copy操作

![](/img/ios/block/blockcopy.png)

![](/img/ios/block/duizhan.png)

一个对象的成员变量是一个block,在栈上面创建这个blcok,如果我们没有使用copy,当我们通过具体的成员变量访问对应blcok,可能由于栈对应的函数退出之后，在内存中就销毁掉了，再继续访问就会引起内存崩溃



**__forwarding存在的意义**

![](/img/ios/block/copydui.png)

栈上的forwarding指针指向自身，block经过copy之后，栈上的forwarding指针指向堆上的__block变量，堆上的forwarding指针指向自身

不论在任何位置，都可以顺利的访问同一个__blcok 变量


#### block 循环引用问题

**思考下面代码有什么问题 如何解决**
``` 
{
    _array = [NSMutableArray arrayWithObjcet:@"123"];
    _strBlock = ^NSString*(NSString* str){
        return [NSString stringWithFormat:@"%@_%@",str,_array[0]];
    }
    _strBlock(@"hello");
}
``` 
引起自循环引用，当前对象强引用block,block表达式中又使用了当前对象的成员变量array，在截获变量中连同修饰符权限共同截获
所以就有一个strong类型的指针指向当前对象，所以产生了，循环引用

使用__weak NMutableArray *weakarray = _array 解除循环引用

**思考下面代码有什么问题 如何解决**

``` 
{
    __block TestBlock* blockSelf = self;
    _blk = ^int(int num) {
        int result = num *blockSelf.var

        return result;
    }
    _blk(3)
}
``` 
在MRC不会有任何问题

ARC情况下

![](/img/ios/block/arcyy.png)

解决办法在block表达式中 blockSelf = nil

