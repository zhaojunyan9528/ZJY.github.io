---
title: promise理解
tags:
  - 前端
categories:
  - - 面试
    - Javascript
date: 2021-01-20 17:15:47
---

## promise

Promise 是异步编程的一种解决方案
Promise 是一个对象，从它可以获取异步操作的消息

Promise对象有以下两个特点。
+ 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功
）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

+ 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

promise的缺点：
+ 1.无法取消Promise，一旦新建它就会立即执行，无法中途取消。
+ 2.如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
+ 3.当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

promise是一个构造函数，用来生成promise实例

```javascript
const promise = new Promise(function(resolve,reject){
    if(true){
        resolve(value)
    }else{
        reject(error)
    }
})
```

promise构造函数接收一个函数作为参数，该函数等两个参数为resovle和reject，由js引擎提供，不用自己部署

resovle函数的作用是，将promise对象的状态从“未完成”变为“成功”（pending变为resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。

reject函数等作用是，将promise对象的状态从“未完成”变为“失败”（pending变为rejected），在异步操作失败时调用，并将异步操作返回的结果作为参数传递出去

promise生成以后，可用then方法分别指定为resolve和reject状态的回调函数：

promise.then(function(value){},function(error){})

then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。