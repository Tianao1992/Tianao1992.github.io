---
title: 数据结构-栈
date: 2020-3-23 10:58:27
index_img: /img/bg/datastructures.jpg
categories:
- 数据结构
tags:
- datastructure
---

### 栈的介绍

它是一种运算受限的线性表后进先出（LFIO）。限定仅在表尾进行插入和删除操作的线性表。这一端被称为栈顶，相对地，把另一端称为栈底。向一个栈插入新元素又称作进栈、入栈或压栈，它是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素

#### 栈的示意图

![](/img/dataStructures/stack.png)

#### 栈题目

**根据六个元素6,5,4,3,2,1的顺序进栈哪一个不是合法出栈的序列**

A 5,4,3,6,1,2   

B 4,5,3,2,1,6  

C 3,4,6,5,2,1  

D 2,3,4,1,5,6

答案是 c

- A 65入5出，4入4出 3入3出 6出 21入 1出 2出

- B 65入4出5出 3入3出 2入2出 1入1出 6出

- C 6543入 3 出 4出 应该是5出 所以不符合

- D 65432入 2出 3出 4出 1入1出 5出 6出


**十进制 转化成二进制**

js 根据数组封装一个栈

```
export class Stack {
    constructor() {
        this.items = []
    }
    push(element) {
        this.items.push(element)
    }
    
    pop() {
        return this.items.pop()
    }

    peek() {
        return this.items[this.items.length-1]
    }
    isEmpty() {
        return this.items.length === 0
    }
    size() {
        return this.items.length
    }
}
```

具体方法实现
```
export function dec2bin(num) {
    const stack = new Stack()
    // 循环取余数
    while(num > 0) {
        let remainder = num % 2
        num = Math.floor(num / 2)
        stack.push(remainder)
    }
    // 拼接字符串
    let binString = ""
    while(!stack.isEmpty()) {
        binString += stack.pop()
    }
    return binString
}

```

