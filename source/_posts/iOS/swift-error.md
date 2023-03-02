---
title: 错误处理
date: 2021-02-04 10:02:39
index_img: /img/ios/swift/swift.png
categories:
- Swift
tags:
- ios
---

### 错误类型
常见错误类型语法错误、逻辑错误、运行时错误

swift中可以通过Error协议自定义错误信息
```
enum SomeError: Error {
    case illeag(String)
    case outOfBund(Int,Int)
    case outMermey
}
```
内部通 throw抛出错误
```
func  divdi(_ num1: Int ,_ num2: Int) throws -> Int{
    //处理不合理函数
    if num2 == 0 {
        //抛出错误
        throw  SomeError.illeag("o不能作为整数")
    }
}
```

### do-catch 捕捉错误
```
func test(){
    do {
       print( try divdi(20, 0))
    } catch let SomeError.illeag(msg) {
        print("参数错误",msg)
    }catch let SomeError.outOfBund(size, index) {
        print(size,index)
    }catch let SomeError.outMermey{
        print("内存泄漏")
    }catch{
        print("其他错误")
    }
    do {
        print( try divdi(20, 0))
    } catch let error as SomeError {
        print(error)
    }catch {
        print("其他错误")
    }
}
test()
```

### try? try! 

使用try? try! 处理可能抛出Error错误的函数，这样就不处理error

```
func  divdi(_ num1: Int ,_ num2: Int) throws -> Int{
    //处理不合理函数
    if num2 == 0 {
        //抛出错误
        throw  SomeError.illeag("o不能作为整数")
    }
    return num1 / num2
}

var result = try? divdi(10, 0)
```
### rethrows
闭包参数引起的错误
```
func exec(_ fn:(Int,Int) throws -> Int, _ num1: Int, _ num2: Int) rethrows {
    print(try fn(num1,num2))
}
try exec(divdi, 20, 10)
```

### defer
代码块结束之前必须执行,将延迟当前作用域结束之前执行
```
func test()  {
    defer {
        print("ee")
    }
   print(try divdi(20, 0))
}
```

### fatalError

如果遇到严重问题，希望结束程序运行，可以直接使用fatalError抛出错误

```
func test(_ num: Int) -> Int {
    if num >= 0 {
        return 1
    }
    fatalError("num不小于9")
}
```
