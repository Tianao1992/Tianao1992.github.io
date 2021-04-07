---
title: 数据结构-链表
date: 2020-3-24 09:02:32
index_img: /img/bg/datastructures.jpg
categories:
- 数据结构
tags:
- datastructure
---

### 链表

链表是一种非常常见的用于存储线数据的线性结构，链表和数组一样，可以用于存储一系列的元素，但链表和数组的实现机制完全不同

### 链表和数组的缺点

 要存储多个元素，数组可能是最常用的数据结构，几乎每一种编程语言都有默认实现数组结构

 **数组的缺点**

- 创建一个数组通常需要申请一段连续的内存的空间，并且大小是固定（大多数语言，所以当前数组不能满足容量的需求是，需要扩容（一般情况是申请一个更大的数组，比如2倍，然后将原数数组的元素复制过去）。

- 数组开头或者中间位置插入数据的成本很高，需要进行大量元素的位移

**相对于数组，链表有哪些优点**

- 内存空间不是必须连续的，可以充分利用好计算机的内存，实现灵活的内存动态管理

- 链表不必再创建时就确定大小，并且大小可以无限延伸下去

- 链表在插入和删除的数据时，时间复杂度可以达到O(1),相对数组效率高很多

### 单向链表

单向链表(单链表)是链表的一种，它由节点组成，每个节点都包含下一个节点的指针，head指针指向第一个节点称为表头，而终止于最后一个指向nuLL的指针

#### 单向链表示意图

![](/img/dataStructures/linklist.jpg)

**js单向链表节点Node封装**

```
// 封装节点类
export class Node {
    constructor(data) {
      this.data = data;
      this.next = null;
    }
  }
```

**js单向链表封装**

```
 // 单向链表结构的封装
  export class LinkedList {
  
    constructor() {
      // 初始链表长度为 0
      this.length = 0;
  
      // 初始 head 为 null，head 指向链表的第一个节点
      this.head = null;
    }
  
    // ------------ 链表的常见操作 ------------ //
  
    // append(data) 往链表尾部追加数据
    append(data) {
      // 1、创建新节点
      const newNode = new Node(data);
  
      // 2、追加新节点
      if (this.length === 0) {
        // 链表长度为 0 时，即只有 head 的时候
        this.head = newNode;
      } else {
        // 链表长度大于 0 时，在最后面添加新节点
        let currentNode = this.head;
  
        // 当 currentNode.next 不为空时，
        // 循序依次找最后一个节点，即节点的 next 为 null 时
        while (currentNode.next !== null) {
          currentNode = currentNode.next;
        }
  
        // 最后一个节点的 next 指向新节点
        currentNode.next = newNode;
      }
  
      // 3、追加完新节点后，链表长度 + 1
      this.length++;
    }
  
    // insert(position, data) 在指定位置（position）插入节点
    insert(position, data) {
      // position 新插入节点的位置
      // position = 0 表示新插入后是第一个节点
      // position = 1 表示新插入后是第二个节点，以此类推
  
      // 1、对 position 进行越界判断，不能小于 0 或大于链表长度
      if (position < 0 || position > this.length) return false;
  
      // 2、创建新节点
      const newNode = new Node(data);
  
      // 3、插入节点
      if (position === 0) {
        // position = 0 的情况
        // 让新节点的 next 指向 原来的第一个节点，即 head
        newNode.next = this.head;
  
        // head 赋值为 newNode
        this.head = newNode;
      } else {
        // 0 < position <= length 的情况
  
        // 初始化一些变量
        let currentNode = this.head; // 当前节点初始化为 head
        let previousNode = null; // head 的 上一节点为 null
        let index = 0; // head 的 index 为 0
  
        // 在 0 ~ position 之间遍历，不断地更新 currentNode 和 previousNode
        // 直到找到要插入的位置
        while (index++ < position) {
          previousNode = currentNode;
          currentNode = currentNode.next;
        }
  
        // 在当前节点和当前节点的上一节点之间插入新节点，即它们的改变指向
        newNode.next = currentNode;
        previousNode.next = newNode;
      }
  
      // 更新链表长度
      this.length++;
      return newNode;
    }
  
    // getData(position) 获取指定位置的 data
    getData(position) {
      // 1、position 越界判断
      if (position < 0 || position >= this.length) return null;
  
      // 2、获取指定 position 节点的 data
      let currentNode = this.head;
      let index = 0;
  
      while (index++ < position) {
        currentNode = currentNode.next;
      }
  
      // 3、返回 data
      return currentNode.data;
    }
  
    // indexOf(data) 返回指定 data 的 index，如果没有，返回 -1。
    indexOf(data) {
      let currentNode = this.head;
      let index = 0;
  
      while (currentNode) {
        if (currentNode.data === data) {
          return index;
        }
        currentNode = currentNode.next;
        index++;
      }
  
      return -1;
    }
  
    // update(position, data) 修改指定位置节点的 data
    update(position, data) {
      // 涉及到 position 都要进行越界判断
      // 1、position 越界判断
      if (position < 0 || position >= this.length) return false;
  
      // 2、痛过循环遍历，找到指定 position 的节点
      let currentNode = this.head;
      let index = 0;
      while (index++ < position) {
        currentNode = currentNode.next;
      }
  
      // 3、修改节点 data
      currentNode.data = data;
  
      return currentNode;
    }
  
    // removeAt(position) 删除指定位置的节点，并返回删除的那个节点
    removeAt(position) {
      // 1、position 越界判断
      if (position < 0 || position >= this.length) return null;
  
      // 2、删除指定 position 节点
      let currentNode = this.head;
      if (position === 0) {
        // position = 0 的情况
        this.head = this.head.next;
      } else {
        // position > 0 的情况
        // 通过循环遍历，找到指定 position 的节点，赋值到 currentNode
  
        let previousNode = null;
        let index = 0;
  
        while (index++ < position) {
          previousNode = currentNode;
          currentNode = currentNode.next;
        }
  
        // 巧妙之处，让上一节点的 next 指向到当前的节点的 next，相当于删除了当前节点。
        previousNode.next = currentNode.next;
      }
  
      // 3、更新链表长度 -1
      this.length--;
  
      return currentNode;
    }
  
    // remove(data) 删除指定 data 的节点，并返回删除的那个节点
    remove(data) {
      return this.removeAt(this.indexOf(data));
    }
  
    // isEmpty() 判断链表是否为空
    isEmpty() {
      return this.length === 0;
    }
  
    // size() 获取链表的长度
    size() {
      return this.length;
    }
  
    // toString() 链表数据以字符串形式返回
    toString() {
      let currentNode = this.head;
      let result = '';
  
      // 遍历所有的节点，拼接为字符串，直到节点为 null
      while (currentNode) {
        result += currentNode.data + ' ';
        currentNode = currentNode.next;
      }
  
      return result;
    }
  }
  
```

### 链表翻转（常见面试问题）

#### 递归翻转

![](/img/dataStructures/digulinklist.png)

具体代码实现

```
function reversList(head) {
    //设置递归出口
  if(head == null || head.next == null) {
      return head
  }
   let newNode = reversList(head.next)
   head.next.next = head // 4 -> 5
   head.next = null // 5 -> null
  return newHead
}

```

#### 迭代翻转

![](/img/dataStructures/diedailinklist.png)

```
function reversList(head) {
  if(head == null || head.next == null) {
      return head
  }
   let newHead = null 
   while(head != null) {
       let tmp = head.next
       head.next = newHead
       newHead = head
       head = tmp
   }
  return newHead
}
```

### 双向链表

双向链表顾名思义，就是链表由单向的链变成了双向链。 使用这种数据结构，我们可以不再拘束于单链表的单向创建于遍历等操作，大大减少了在使用中存在的问题

#### 双向链表示意图

![](/img/dataStructures/doublinklist.png)


#### 双向链表的特点

- 可以使用一个head 和一个 tail 分别指向头部和尾部的特点

- 每个节点都有三部分注册 前一个节点的指针（prev),data,后一个节点指针（next）

- 双向链表的第一个节点的prev是null

- 双向链表的最后的节点的next是null


#### js 双向链表封装

```
//继承单节点
class DoublyNode extends Node {
    constructor(element) {
        super(element)
        this.prev = null 
    }
}

export class DoublyLinkList extends LinkedList {
   constructor() {
       super()
       this.tail = null
   }
   //拼接节点
    append(element) {
        const newNode = DoublyNode(element)
        if (this.head == null) {
            this.head  = newNode
            this.tail = newNode
        }else{
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
        }
        this.length++
    }
    //插入节点
    insert(position,element) {
     if (position < 0 || position > this.length) return false;
      const newNode = DoublyNode(element)
      if (position ===  0) {
         if (this.head == null) {
            this.head = newNode
            this.tail = newNode
         }else{
             newNode.next = this.head
             this.head.prev = newNode
             this.head = newNode
         }
         
      }else if (position ==   this.length){
        this.tail.next = newNode;
        newNode.prev = this.tail
        this.tail = newNode
      }else {
          let index = 0
          let currrent = this.head
          let preverd  = null
          while(index++ < position) {
              preverd = currrent
              currrent = currrent.next
          }
          newNode.prev = preverd
          preverd.next = newNode

          newNode.next = currrent
          currrent.prev  = newNode
      }

      this.length++
      return true
    }
    //删除节点
    removeAt(position) {
        if (position < 0 || position > this.length - 1)  return null;
        let currrent = this.head
        if (position === 0 ) {
            if (this.length === 1) {
                this.head = null
                this.tail = null
            }else{
                this.head = this.head.next
                this.head.prev = null
            }
        }else if (position === this.length - 1) {
           currrent = this.tail
           this.tail = this.tail.prev
           this.tail.next = null
         
        }else{
            let index = 0
            let preverd  = null
            while(index++ < position) {
                preverd = currrent
                currrent = currrent.next
            }
            preverd.next = currrent.next
            currrent.next.prev = preverd
        }

        this.length--
        return currrent.element
    }

}
```






