---
title: setTimeout中的this指向
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 17:33:24
---
## setTimeout中的this指向

```js
var x = 1;
var obj = {
    x:2,
    fn:function(){
        console.log(this.x);
        console.log(this);
        console.log(obj)
    }
}
obj.fn(); 
//output:
// 2
// {
//     x:2,
//     fn:f()
// }
// {
//     x:2,
//     fn:f()
// }


setTimeout(obj.fn, 1000);
// output
//1
//window全局对象
// {
//     x:2,
//     fn:f()
// }
//等同于：setTimeout是window下的方法
window.setTimeout(obj.fn, 1000);
// obj.fn相当于把fn函数给拿出来调用
window.setTimeout(function(){
        console.log(this.x);//this是全局对象
        console.log(this);//全局
        console.log(obj);//obj
}, 1000);
```
