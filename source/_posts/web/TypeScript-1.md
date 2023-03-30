---
title: TypeScript_基础语法
date: 2023-03-02 13:36:38
index_img: /img/h5/ts_logo.png
categories:
- 前端
tags:
- H5
---

## TypeScript 是什么？
官方介绍：TypeScript 是 JavaScript 的超集，这意味着它可以完成 JavaScript 所做的所有事情而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。  

TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。下图显示了 TypeScript 与 ES5、ES2015 和 ES2016 之间的关系:
![](/img/h5/ts_img/ts_chaoji.png)

#### 1.1 TypeScript 与 JavaScript 的区别
> 如图
![](/img/h5/ts_img/ts_js_qubie.png)

#### 1.2 TypeScript 大致工作流程
> 如图
![](/img/h5/ts_img/ts_work.jpeg)
如图所示ts 包含 a.ts b.ts c.ts三个ts 文件最终会被TypeScript编译器根据配置的编译选项生成 a.js b.js c.js最终通过打包工具对生成的js文件进行打包处理，然后进行部署

## TypeScript 基础类型

- 1. 布尔类型
```
 let a :boolean = true 

```
- 2. number类型
```
let b: number = 2

```
- 3. 字符串类型
```
let c: string = ""

```