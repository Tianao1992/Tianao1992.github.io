---
title: this 指向问题
date: 2021-01-07 14:12:03
index_img: /img/h5/fronted.jpg
categories:
- 前端
tags:
- H5
---

### JavaScript this 关键字
面向对象语言中 this 表示当前对象的一个引用。
但在 JavaScript 中 this 不是固定不变的，它会随着执行境的改变而改变

**哪个对象调用函数，函数里面的this指向哪个对象**

**可以分为这5种情况讨论**

1. 作为普通函数被调用
2. 作为对象属性被调用
3. 作为构造函数
4. call和apply的应用
5. 箭头函数

### 普通函数被调用

```js
     function test() {
         console.log(this);
     }
  test()// Window
```
### 对象属性被调用

```js
 let obj = {
         id:'123',
         func() {
           console.log(this.id);
           console.log(this);
         },
     }
     obj.func();// 123 obj
```
### 构造函数调用

```js
  let objclass = function() {
         console.log(this);//objclass
    }
    let a = new objclass()
```
在构造函数里面返回一个对象，会直接返回这个对象，而不是执行构造函数后创建的对象

```js

     let objclass = function() {
         this.a ='1234'
          return {
              b:'456'
          }
     }
     let item  = new objclass()
     console.log(item.a)//undefined
```

### call和apply 调用

apply和call简单来说就是会改变传入函数的this指向

```js
let obj1 =  {
         name:'zhangsan'
     }
     let obj2 = {
         name: 'lisi',
         fn() {
            console.log(this.name);
         }
     }
     obj2.fn.call(obj1) // zhangsan
```
call 和 apply 两个主要用途：

- 改变 this 的指向（把 this 从 obj2 指向到 obj1 ）

- 方法借用（ obj1 没有 fn ，只是借用 obj2 方法）


###  箭头函数调用

箭头函数没有this，这里的this引用的是最近作用域(aaa函数里的this)的this

```js
 const obj = {
      aaa() {
        setTimeout(function () {
          setTimeout(function () {
            console.log(this) //window
          })
          setTimeout(() => {
            console.log(this) //window
          })
        })
        setTimeout(() => {
          setTimeout(function () {
            console.log(this) //window
          })
          setTimeout(() => {
            console.log(this) //obj
          })
        })
      }
    }
    obj.aaa()
```










