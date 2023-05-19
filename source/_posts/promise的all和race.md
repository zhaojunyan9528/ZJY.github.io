---
title: promise的all和race
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2023-05-19 11:26:22
---

## 记Promise.all和race方法使用

### Promise.all

Promise.all()方法接收一个promise的iterable类型（注：Array, Map, Set都属于es6的iterable类型）的输入，并返回一个Promise实例，那么输入的所有promise的resolve回调的结果是一个数组。
这个Promise的resolve回调执行是在所有输入的promise的resolve回调都结束，或者输入的iterable里没有promise了的时候。
他的reject回调执行时，只要任何一个输入的promise的reject回调执行或者输入不合法的promise就会立即抛出错误，并且reject的是第一个抛出的错误信息。

***Promise.all 等待所有都完成或第一个失败。***

```js
var p1 = Promise.resolve(3);
var p2 = 1337;
var p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([p1, p2, p3]).then(values => {
  console.log(values); // [3, 1337, "foo"]
});
```

p2非promise值
***如果请求参数中包含非promise值，这些值会被忽略，但仍然返回在数组中。***

```js
var p = Promise.all([1,2,3]);
var p2 = Promise.all([1,2,3, Promise.resolve(444)]);
var p3 = Promise.all([1,2,3, Promise.reject(555)]);

setTimeout(function(){
    console.log(p);
    console.log(p2);
    console.log(p3);
});

Promise {<fulfilled>: Array(3)}
Promise {<fulfilled>: Array(4)}
Promise {<rejected>: 555}
```

只要任何一个输入的promise的reject回调执行,就会reject

***Promise.all的异步和同步：***

如果传入的可迭代对象是空的，就是同步：

```js
var p = Promise.all([])
console.log(p)
// Promise {<fulfilled>: Array(0)}
```

异步(***即使参数不是promise值，也是异步的***):

```js
var resolvedPromisesArray = [Promise.resolve(33), Promise.resolve(44)];

var p = Promise.all(resolvedPromisesArray);
console.log(p);

// using setTimeout we can execute code after the stack is empty
setTimeout(function(){
  console.log(p);
});

// Promise { <state>: "pending" }
// Promise { <state>: "fulfilled", <value>: Array[2] }

var p = Promise.all([1,2,3]);
console.log(p)
// Promise {<pending>}
```

如果 Promise.all 失败，也是一样的异步:

```js
var mixedPromisesArray = [1, Promise.resolve(33), Promise.reject(44)];
var p = Promise.all(mixedPromisesArray);
console.log(p);
setTimeout(function(){
    console.log(p);
});

// Promise {<pending>}
{/* Promise {<rejected>: 44} */}
```

***Promise.all 的快速返回失败行为***
Promise.all 在任意一个传入的 promise 失败时返回失败

```js
var p1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 1000, 'one');
});
var p2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 2000, 'two');
});
var p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 3000, 'three');
});
var p4 = new Promise((resolve, reject) => {
  setTimeout(resolve, 4000, 'four');
});
var p5 = new Promise((resolve, reject) => {
  reject('reject');
});

Promise.all([p1, p2, p3, p4, p5]).then(values => {
  console.log(values);
}, reason => {
  console.log(reason)
});

//From console:
//"reject"

//You can also use .catch
Promise.all([p1, p2, p3, p4, p5]).then(values => {
  console.log(values);
}).catch(reason => {
  console.log(reason)
});

//From console:
//"reject"
```

需要特别注意的是，Promise.all获得的成功结果的数组里面的数据顺序和Promise.all接收到的数组顺序是一致的，即p4的结果在前，即便p4的结果获取的比p1要晚。

```js
var p1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'one');
});
var p2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 200, 'two');
});
var p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 300, 'three');
});
var p4 = new Promise((resolve, reject) => {
  setTimeout(resolve, 400, 'four');
});

Promise.all([p4, p1, p2, p3]).then(values => {
  console.log(values); // ['four', 'one', 'two', 'three']
}, reason => {
  console.log(reason)
});
```

### Promise.race

Promise.race(iterable)方法返回一个promise，一旦迭代器中某个promise解决或拒绝，返回的promise就会解决或拒绝。

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'one');
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then((value) => {
  console.log(value); // two
  // Both resolve, but promise2 is faster
})
```

***Promise.race的异步性***

```js
var p = Promise.race([1,2])
console.log(p)
// Promise {<pending>}

var p = Promise.race([Promise.resolve(22),2])
console.log(p)
// Promise {<pending>}
```

setTimeout示例：

```js
var p1 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 500, "one");
});
var p2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, "two");
});

Promise.race([p1, p2]).then(function(value) {
  console.log(value); // "two"
  // 两个都完成，但 p2 更快
});

var p3 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, "three");
});
var p4 = new Promise(function(resolve, reject) {
    setTimeout(reject, 500, "four");
});

Promise.race([p3, p4]).then(function(value) {
  console.log(value); // "three"
  // p3 更快，所以它完成了
}, function(reason) {
  // 未被调用
});

var p5 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 500, "five");
});
var p6 = new Promise(function(resolve, reject) {
    setTimeout(reject, 100, "six");
});

Promise.race([p5, p6]).then(function(value) {
  // 未被调用
}, function(reason) {
  console.log(reason); // "six"
  // p6 更快，所以它失败了
});
```
