---
title: vuex
date: 2020-5-07 12:59:13
index_img: /img/h5/vue.jpg
categories:
- 前端
tags:
- vue
---

### 什么是Vuex

Vuex就是一个状态管理模式，为什么叫模式？因为Vuex包含了一套对state(状态)的操作规范，集中存储管理应用的所有组件的状态。

- 简单来说就是管理各个组件共享的数据，类似session

- session可以存数据，存的过程就是管理，数据的每一次赋值就是当次状态。

- Vuex在Vue实例顶层中。

  Vuex也可以理解为java中的一个map，这个map是static（静态资源）的，每次获取map的值，都需要调用java的api，比如map.get(key)获取对应的值，也可以放入数据map.put(data)，而且这个数据是所有类都可以调用的，只需要导入这个map就能使用里面的共享数据。

  不了解java也没关系，你也可以理解成为百度百科就是一个容纳百科知识的容器，你要搜vuex，百科就会出现vuex的描述，这个搜索就是获取状态，如果你角色百科的vuex描述有误。你也可以发起修改操作，改成正确的vuex描述，这个修改操作就是修改vuex在百科里面的状态。当然你可以搜索修改vuex，别人也可以，因为这个状态是共享的。

  简单来看实现这个功能好像我们自己封装一个对象就能实现，但是Vuex有一个特性就是响应式。如果我们自己封装对象想做到完美响应式比较麻烦，所有Vuex帮我们做了这个事情。

> 什么状态需要Vuex去管理？

- 比如用户的登录的状态（token）、用户的信息（头像、名称、地理位置信息）等等
- 比如商品的收藏，购物车的商品等等
- 这些状态应该是响应式的，用户昵称、头像修改了需要响应

![](/img/h5/vue/vuex/19-01.png)

- **state**，驱动应用的数据源；
- **view**，以声明方式将 **state** 映射到视图；
- **actions**，响应在 **view** 上的用户输入导致的状态变化。


这是一个单页面数据流向，比如想要修改用户昵称，当前用户昵称的状态是A，通过输入框输入了新的昵称B，调用ajax请求后端修改成功后将state改成B，然后视图响应用户昵称的变化从A到B。

但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏：

- 多个视图依赖于同一状态。
- 来自不同视图的行为需要变更同一状态。
- 所以我们需要vuex的规范操作来管理状态。


### Vuex的核心概念

- State
- Getters
- Mutation
- Action
- Moudule

### Vuex的流程

![](/img/h5/vue/vuex/19-03.png)

- Vue Components是vue组件
- Mutations ：更改 Vuex 的 store 中的状态的唯一方法是提交 mutation
- State 是vuex中状态的集合
- Actions与Mutations 类似，经常与后端交互，不同在于：
  - Action 提交的是 mutation，而不是直接变更状态。
  - Action 可以包含任意异步操作。

组件中修改state，通过提交 mutation，修改完成后vuex帮我们响应到vue组件上。



> 修改index.js使用mutation

```js
// 2.创建对象
const store = new Vuex.Store({
  state: { // 状态集合
    count: 0 // 具体的状态数据
  },
  mutations: { // 操作修改state（状态）
    increment (state) { // 增加
      state.count++
    },
    decrement (state) { // 减少
      state.count--
    }
  }
})
```

```vue
<template>
  <div id="app">
    <h3>{{ message }}</h3>
    <h3>{{ $store.state.count }}</h3>
    <button @click="add">+</button>
    <button @click="sub">-</button>
    <hello-vuex />
  </div>
</template>

<script>
import HelloVuex from './components/HelloVuex'

export default {
  name: 'App',
  data () {
    return {
      message: ''
    }
  },
  components: {
    HelloVuex
  },
  methods: {
    add () {
      this.$store.commit('increment')
    },
    sub () {
      this.$store.commit('decrement')
    }
  }
}
</script>
```
###  State（单一状态树）
Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 ([SSOT](https://en.wikipedia.org/wiki/Single_source_of_truth))”而存在。。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

简单说就是把数据所有有关的数据封装到一个对象中，这个对象就是store实例，无论是数据的状态（state），以及对数据的操作（mutation、action）等都在store实例中，便于管理维护操作。
直接通过`this.$store.state`获取state对象。


### Mutation（状态更新）

- Vuex的store状态更新的唯一方式：**提交Mutation**

- Mutation主要包括两个部分：

  1. 字符串的**事件类型（type）**
  2. 一个**回调函数（handler）**，这个回调函数就是我们实际进行状态更改的地方，该回调函数的**第一个参数就是state**

    ```js
  mutation: {
      increment (state) {
          state.count++
      }
  }
  ```

- 通过Mutation更新

  ```js
  mutation () {
  	this.$store.commit('increment')
  }
  ```

#### mutation 提交风格

1. 普通提交风格

   ```js
   this.$store.commot('increment', count)
   ```

   此时count传过去的就是count=10

2. 特殊的提交封装

   ```js
   this.$store.commit({
   	type: 'addCount',
   	count
   })
   ```

   ```js
   addCount (state, payload) { // 此时传入的就不是一个count值了，而是一个对象
   	state.count += payload.count
   }
   ```

   此时count传过去是一个对象payload（载荷）

   ```js
   {
   	type: 'incrementCount',
   	count: 10
   }
   ```
#### actions

处理异步操作，如网络请求等，然后再使用mutation操作更新


Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。


1. 新增一个mutation

```js
updateName (state, name) {
	state.user.name = name
}
```

2. 新增一个actions

```js
actions: {
    // context：上下文
    aUpdateInfo (context, name) {
        setTimeout(() => {
        context.commit(updateName, 12)
        }, 1000)
    }
}
```

3. 提交方法

```js
aUpdateInfo () {
	this.$store.dispatch('aUpdateInfo', 'lisi')
}
```
点击按钮之后，信息修改了，dev-tools也能跟踪到state的变化。通过`$store.dispacth()`方法来调用actions，发送异步请求，在actions中需要提交mutation来修改state。


### moudules（模块）


由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。


```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状
```



#### 项目结构

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

只要你遵守以上规则，如何组织代码随你便。如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```
