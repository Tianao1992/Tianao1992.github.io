---
title: oc与swift混编
date: 2021-02-05 15:50:06
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### swift 调用 oc

- 创建oc 文件默认生成 xxxx-Bridging-Header.h头文件

- 将 xxxx-Bridging-Header.h中 #import 暴露给swift 内容

#### swift调用oc- TestPerson.h .m

```
int sum (int a, int b);
void testSwift();

@interface TestPerson : NSObject
@property (nonatomic,assign) NSInteger age;
@property (nonatomic,copy) NSString *name;
- (instancetype)initWithAge:(NSInteger)age name:(NSString *)name;
+ (instancetype)personWithAge:(NSInteger)age name:(NSString *)name;
-(void)run;
+(void)run;
- (void)eat:(NSString *)food other:(NSString *)other;
+(void)eat:(NSString *)food other:(NSString *)other;
@end
```


```
#import "TestPerson.h"
@implementation TestPerson
- (instancetype)initWithAge:(NSInteger)age name:(NSString *)name{
    if (self = [super init]) {
        self.age = age;
        self.name = name;
    }
    return  self;
}

+ (instancetype)personWithAge:(NSInteger)age name:(NSString *)name{
    return  [[self alloc] initWithAge:age name:name];
}
- (void)eat:(NSString *)food other:(NSString *)other{
    NSLog(@"实例方法food");
}

+ (void)eat:(NSString *)food other:(NSString *)other{
    NSLog(@"类方法");
}

- (void)run{
    NSLog(@"实例 run");
}
+ (void)run {
    NSLog(@"person run");
}
@end
int sum (int a, int b) { return  a + b; };
```

调用
```
var person = TestPerson(age: 20, name: "zhangsan")
person.age = 20
person.name = "lisi"
person.run()
person.eat("mian", other: "shuiguo")
TestPerson.eat("min", other: "qi")
TestPerson.run()


print(sum(10, 20))
```

#### @_silgen_name

如果c语言暴露给swift，与swift中其他函数名冲突
可以使用@——sigen_name

```
int sum (int a, int b) { return  a + b; };
@_silgen_name("sum") func swift_num(v1: Int,v2: Int) -> Int
```

### oc调用swift

xcode 默认生成oc调用swift头文件"xxx-Swift.h"

- swift暴露给oc的类最终继承与NSObject

- @objc修饰需要暴露给oc成员

- @objcMembers默认全部暴露给oc
//@objc 单个暴露给oc 全员暴露给 oc
```
@objcMembers class Car: NSObject {
    var price: String
    var bind: String
    init(price: String ,bind: String) {
        self.price = price
        self.bind = bind

    }
    //希望走runtime 加关键字 dynamic
     func run() {
       print("car run")
    }

    static func run() {
        print("Car run")
    }

}

```

```

void testSwift() {
    Car *car = [[Car alloc] initWithPrice:@"2200" bind:@"123"];
    [car run];
    car.price = @"19999";    
    [car test];
}

```
### oc 调用swift组件库
 例如 调用CTMediatorswift第三方库   @import CTMediator 即可

### swift 闭包转换成 oc 调用的block 
这与 Swift 如何实现闭包有关。您需要使用@convention(block)来注释闭包是 ObjC 块。使用unsafeBitCast武力投它
var block : @convention(block) (NSString!) -> Void = {
   (string : NSString!) -> Void in 
    println("test")
}
ctx.setObject(unsafeBitCast(block, AnyObject.self), forKeyedSubscript: "test")


#### 选择器
swift中仍然可以使用选择器#selector定义一个选择器
但必须被@objcMembers修饰或者@objc修饰方法
```
@objcMembers class Car: NSObject {

    func test(v1: Int)  {
        print(v1)
    }
    func test1(v1:Int,v2: Int)  {
        
    }
    
    func run() {
        perform(#selector(test(v1:)))
        perform(#selector(test1(v1:v2:)))
    }

}
```

### String

swift中字符串类型String与oc的NSString,api上有比较大的差异
```
var str: String = "1"
str.append("2")
str = "\(str)_5"

var str1 = "123456"
print(str1.hasPrefix("123"))//true
print(str1.hasSuffix("456"))//true
```

#### String 插入删除

```
var str = "1_2"
str.insert("_", at: str.endIndex)//"1_2_"

str.insert(contentsOf: "3_4", at: str.endIndex)//"1_2_3_4"

str.insert(contentsOf: "666", at: str.startIndex)//"1666_2_3_4"
```

```
str.remove(at: str.firstIndex(of: "1")!)//"666_2_3_4"
str.removeAll{$0 == "6"}//"_2_3_4"
var rang = str.index(str.endIndex,offsetBy: -2) ..< str.index(before: str.endIndex)
str.removeSubrange(rang)//_2_34
```
### Substring

![](/img/ios/swift/class/substing.png)
### swift、oc桥接转换表

![](/img/ios/swift/class/zhuanhuan.png)

### 可选协议
可以通过@objc定义可选协议，这种协议只能被class遵守

```

@objc protocol TestProtocol{
    func test()
   @objc optional func run()
}

```

### dynamic
被@objc dynamic修饰内容具有动态性，比如方法会走runtime那一套
```
class Dog: NSObject {
    @objc dynamic func test1() {}
}
var d = Dog()
d.test1()

```

### KVC\KVO
swift支持添加kvc\kvo
属性所在的类、监听器最终继承与NSObjcet
被@objc dynamic修饰
```
class Observer: NSObject {

    override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {

        print("observeValue",change?[.newKey] as Any)
    }
}
class Person: NSObject {
    @objc dynamic var age: Int = 10
    var observer:Observer = Observer()
    override init() {
        super.init()
        self.addObserver(observer, forKeyPath: "age", options: .new, context: nil)
    }
    deinit {
        self.removeObserver(observer, forKeyPath: "age")
    }
}

var p = Person()
p.age  = 20
```

### 关联对象

swift中，class仍然可以使用关联对象

- 默认情况 extension不可以增加存储属性

- 借助关联对象可以实现extension增加存储属性
```
class Person {

}
extension Person {
    private static var AGE_KEY: Bool = false
    var age: Int {
        get {
            objc_getAssociatedObject(self, &Person.AGE_KEY) as! Int
        }
        set {
            objc_setAssociatedObject(self, &Person.AGE_KEY, newValue, .OBJC_ASSOCIATION_ASSIGN)
        }
    }
}
var p = Person()
p.age = 10
```

### 多线程

```
public typealias Task = () -> Void

public struct Async {
    public static func async(_ task: @escaping Task){
        _async(task)
    }
    
    public static func async(_ task: @escaping Task,  _ mainTask: @escaping Task) {
        _async(task, mainTask)
    }
    
    
    private static func  _async(_ task:  @escaping Task, _ mainTask: Task? = nil) {
        let item = DispatchWorkItem(block: task)
        DispatchQueue.global().async(execute: item)
        if let main = mainTask {
            item.notify(queue: DispatchQueue.main, execute: main)
        }
    }
    
    public static func asyncDelay(_ second: Double, _ task: @escaping  Task, _ mainTask: @escaping Task) -> DispatchWorkItem {
        _asyncDelay(second, task, mainTask)
    }
    
    private static func  _asyncDelay(_ second: Double, _ task: @escaping Task,  _ mainTask: Task? = nil) -> DispatchWorkItem {
        let itme = DispatchWorkItem(block: task)
        
        DispatchQueue.global().asyncAfter(deadline: DispatchTime.now() + second, execute: itme)
        if let main = mainTask {
            itme.notify(queue: DispatchQueue.main, execute: main)
        }
        return itme
    }
    
    
    
}

```


//加锁
```
   private static var data = [String: Any]()
    
    //信号量加锁
//    private static var lock = DispatchSemaphore(value: 1)
    
    private static var lock = NSRecursiveLock()
    
    
    static func set (_ key: String, value: Any) {
//        lock.wait()
//        defer {
//            lock.signal()
//        }
//        data[key] = value
        
        lock.lock()
        defer {
            lock.unlock()
        }
        data[key] = value
       
    }
```