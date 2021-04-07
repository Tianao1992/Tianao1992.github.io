---
title: 常见排序
date: 2020-05-31 17:51:20
index_img: /img/bg/tree.jpg
categories:
- ItTree
tags:
- basic
---


### 冒泡排序

#### 介绍
 冒泡排序是一种简单直观的排序算法，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢"浮"到数列的顶端

#### 步骤
- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。

- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
- 针对所有的元素重复以上的步骤，除了最后一个。
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较

#### 演示
![](/img/ittree/sort/bubbleSort.gif)

#### 代码实现

```js
 function BubbleSort(arr) {
  for (let  i = 0; i < arr.length - 1; i++) {
     let bool = false
        for (let j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j + 1] < arr[j]) {
            let tmp = arr[j]
            arr[j] = arr[j + 1]
            arr[j + 1] = tmp
            bool = true 
           }
        }
        if (bool == false ) {
              break
          }
 	    }
 	   return arr
 	}
```


### 选择排序

#### 介绍

选择排序是一种简单直观的排序算法
首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
重复第二步，直到所有元素均排序完毕


#### 演示
![](/img/ittree/sort/selectionSort.gif)

#### 代码实现

```js
 function selectionSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        let minindex = i
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[minindex]) {
               	        minindex = j
                }
            }
            if (minindex != i) {
               	   let tmp = arr[minindex]
               	    arr[minindex] = arr[i]
               	   arr[i] = tmp
              }
          }
          return arr
 	 }
```

### 插入排序

#### 介绍
插入排序是一种简单直观的排序算法，工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入
类似于我们生活中打扑克

#### 步骤
- 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。

- 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置

#### 演示
![](/img/ittree/sort/insertionSort.gif)

#### 代码实现

```js
function insertSort(arr) {
 	 	var len = arr.length
 	 	var  preIndex,current
 	 	for (var  i = 1; i < len; i++) {
 	 	     preIndex = i - 1
 	 	     current = arr[i]

 	 	     while(preIndex >= 0 && arr[preIndex] > current) {
 	 	        arr[preIndex + 1] = arr[preIndex];
 	 	        preIndex--;
 	 	     } 
            
           arr[preIndex + 1] = current
 	 	}
 	 	return arr

 	 }
```

### 快速排序

#### 介绍

快速排序使用分治法（Divide and conquer）策略来把一个串行（list）分为两个子串行（sub-lists）。
快速排序又是一种分而治之思想在排序算法上的典型应用。本质上来看，快速排序应该算是在冒泡排序基础上的递归分治法。
快速排序的名字起的是简单粗暴，因为一听到这个名字你就知道它存在的意义，就是快，而且效率高！它是处理大数据最快的排序算法之一了

#### 步骤

1. 从数列中挑出一个元素，称为 "基准"（pivot）;

2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；

3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序

#### 演示

![](/img/ittree/sort/quickSort.gif)

#### 代码实现

```js

 function qiuckSort(arr,left, right) {
 	 	   
          if (left < right) {
          	 let index = getIndex(arr,left,right)
          	 qiuckSort(arr,left,index - 1)
             qiuckSort(arr,index + 1 ,right)   
          }
        return arr
 	 }

 	 function getIndex(arr,low,hight) {
 	 	let pivot = arr[low]

         while(low < hight) {
             while(low < hight && arr[hight] >= pivot) {
             	hight--
             }
             arr[low] = arr[hight]
              while(low <  hight && arr[low] <= pivot) {
             	low++
             }
            arr[hight] = arr[low]
         }
         arr[low] = pivot

         return low
 	 }


```
