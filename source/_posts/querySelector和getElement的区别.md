---
title: querySelector和getElement的区别
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-04-09 14:57:40
---

querySelector()和querySelectorAll()是原生的选择符

+ querySelector 属于 W3C 中的 Selectors API 规范 。而 getElementsBy 系列则属于 W3C 的 DOM 规范"
+ getElementBy系列方法获取的是动态集合，而querySelector获取的是静态集合。
简单来说就是，动态集合选出的元素会随文档改变而改变，但是静态的不会，静态集合在取出来以后元素与文档的改变无关。
+ getElementBy系列方法性能优于querySelector
  
```js
console.time('query');
for(var i=0;i<1000;i++){
document.querySelectorAll('*');
}
console.timeEnd('query');
// query: 77.35107421875 ms

console.time('getElement');
for(var i=0;i<1000;i++){
document.getElementsByTagName('*');
}
console.timeEnd('getElement');
// getElement: 0.22900390625 ms
```