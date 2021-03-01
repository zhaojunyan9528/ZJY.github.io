---
title: javascript的reduce方法
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 14:35:10
---

reduce()方法接收一个函数作为累加器，数组中的每个值从左向右缩减，最终计算为一个值
reduce()作为空数组不会执行回调函数的。

语法:
reduce(function(total,currentValue,currentIndex,arr),initalValue);
参数：

+ function(total,currentValue,currentIndex,arr)：必须，用于执行每个数组元素的函数
  + total:必须，初始值或者用于计算后返回的函数值
  + currentValue:必须，当前值
  + currentIndex：可选，当前元素索引
  + arr: 可选，当前元素所属的数组对象
+ initalValue:可选，传递给函数的初始值
  
返回值：计算结果

```js
[1,2,3].reduce(function(total,num){
    return total + num
})
//6
```