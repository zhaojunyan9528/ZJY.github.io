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

### Promise特点

Promise对象有以下两个特点。

+ 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功
）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

+ 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### Promise的缺点：

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

promise构造函数接收一个函数作为参数，该函数的两个参数为resovle和reject，由js引擎提供，不用自己部署

resovle函数的作用是，将promise对象的状态从“未完成”变为“成功”（pending变为fullfilled），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。

reject函数的作用是，将promise对象的状态从“未完成”变为“失败”（pending变为rejected），在异步操作失败时调用，并将异步操作返回的结果作为参数传递出去

### Promise.then方法

promise生成以后，可用then方法分别指定为resolve和reject状态的回调函数：

promise.then(function(value){},function(error){})

then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。

promise.then(onFulfilled, onRejected)

promise简化了对error的处理，上面的代码我们也可以这样写：

promise.then(onFulfilled).catch(onRejected)

### Promise.all方法

Promise.all 方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
var p = Promise.all([p1,p2,p3]);

上面代码中，Promise.all 方法接受一个数组作为参数，p1、p2、p3 都是 Promise 对象的实例。（Promise.all 方法的参数不一定是数组，但是必须具有 iterator 接口，且返回的每个成员都是 Promise 实例。）

p 的状态由 p1、p2、p3 决定，分成两种情况。

（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

例子：

```js
// 生成一个Promise对象的数组
var promises = [2, 3, 5, 7, 11, 13].map(function(id){
  return getJSON("/post/" + id + ".json");
});
 
Promise.all(promises).then(function(posts) {
  // ...  
}).catch(function(reason){
  // ...
});
```

### Promise.race方法

Promise.race 方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例

var p = Promise.race([p1,p2,p3]);

上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的返回值。

如果Promise.all方法和Promise.race方法的参数，不是Promise实例，就会先调用下面讲到的Promise.resolve方法，将参数转为Promise实例，再进一步处理。


### Promise.resolve 方法

有时需要将现有对象转为Promise对象，Promise.resolve方法就起到这个作用。

```js
var p = Promise.resolve('Hello');
 
p.then(function (s){
  console.log(s)
});
```

如果 Promise.resolve 方法的参数，不是具有 then 方法的对象（又称 thenable 对象），则返回一个新的 Promise 对象，且它的状态为fulfilled。

上面代码生成一个新的Promise对象的实例p，它的状态为fulfilled，所以回调函数会立即执行，Promise.resolve方法的参数就是回调函数的参数。

如果Promise.resolve方法的参数是一个Promise对象的实例，则会被原封不动地返回。

### Promise.reject方法

Promise.reject(reason)方法也会返回一个新的Promise实例，该实例的状态为rejected。Promise.reject方法的参数reason，会被传递给实例的回调函数。

```js
var p = Promise.reject('出错了');
 
p.then(null, function (s){
  console.log(s)
});
// 出错了
```

上面代码生成一个Promise对象的实例，状态为rejected，回调函数会立即执行。

promise实现ajax：

```js
function ajax(URL) {
    return new Promise(function (resolve, reject) {
        var req = new XMLHttpRequest(); 
        req.open('GET', URL, true);
        req.onload = function () {
        if (req.status === 200) { 
                resolve(req.responseText);
            } else {
                reject(new Error(req.statusText));
            } 
        };
        req.onerror = function () {
            reject(new Error(req.statusText));
        };
        req.send(); 
    });
}
var URL = "/try/ajax/testpromise.php"; 
ajax(URL).then(function onFulfilled(value){
    document.write('内容是：' + value); 
}).catch(function onRejected(error){
    document.write('错误：' + error); 
});
```