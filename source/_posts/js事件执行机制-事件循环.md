---
title: js事件执行机制-事件循环
tags:
  - 前端
categories:
  - - 面试
    - Javascript
date: 2021-01-20 11:52:17
---

### 1.javascript是单线程语言

```javascript
let a = 1;
console.log(a);
let b = 2;
console.log(b);
// 1
// 2

setTimeout(function(){
    console.log('定时器开始啦');
})
new Promise(function(resolve){
    console.log('马上开始执行for循环');
    for(var i=0; i<100;i++){
        i==99 && resolve();
    }
}).then(function(){
   console.log('执行then函数');
});
console.log('代码执行结束');
```

结果：
马上开始执行for循环
代码执行结束
执行then函数
定时器开始啦

### 2.Js事件循环

+ 同步任务
+ 异步任务

同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
当指定的事情完成时，Event Table会将这个函数移入Event Queue。
主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
上述过程会不断重复，也就是常说的Event Loop(事件循环)。

```javascript
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
})
console.log('代码执行结束');
```

+ ajax进入Event Table，注册回调函数success。
+ 执行console.log('代码执行结束')。
+ ajax事件完成，回调函数success进入Event Queue。
+ 主线程从Event Queue读取回调函数success并执行

除了广义的同步任务和异步任务，我们对异步任务有更精细的定义：

+ macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
+ micro-task(微任务)：Promise，process.nextTick

事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务

```javascript
console.log('1');

setTimeout(function() {   //宏任务timeout1
    console.log('2');
    process.nextTick(function() { //微任务process2
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() { //微任务then2
        console.log('5')
    })
})
process.nextTick(function() { //微任务process1
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() { //微任务then1
    console.log('8')
})

setTimeout(function() { //宏任务timeout2
    console.log('9');
    process.nextTick(function() { 微任务process3
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() { //微任务then3
        console.log('12')
    })
})
```

宏任务：timeout1—timeout2
微任务：process1—then1—process2—then2—process3—then3
输出顺序：1—7—整个script宏任务执行完毕，执行所有微任务process1，then1，—6—8—执行宏任务timeout1—2—4—执行所有微任务process2，then2—3—5—执行宏任务timout2—9—11—执行所有微任务process3，then3—10—12

1、7、6、8、2、4、3、5、9、11、10、12

事件循环Event loop是js实现异步的一种方法，也是js的执行机制

Javascript是一门单线程语言，事件循环是javascript的执行机制

### 总结

js是单线程非阻塞的脚本语言，意味着代码在js执行的任何时候只有一个主线程来处理所有任务。而非阻塞是指当代码需要异步处理的时候，主线程会挂起这个任务，当异步任务处理完毕后，主线程再根据一定规则去执行相应的回调。

当任务处理完毕，js会将这个事件加入一个队列，这个队列叫做事件队列。被放入事件队列中的事件不会立刻执行其回调，而是等待执行栈中所有任务都执行完毕后，主线程会查询事件队列中是否有任务。

异步任务分为：宏任务和微任务，不同类型的任务会被分配到不同的任务队列中。

当执行栈中所有任务都执行完毕后，会去检查微任务队列是否有事件存在，如果存在，会依次执行任务队列对应的回调，直到为空。然后去宏任务队列中取出一个事件，把对应的回调加入到当前执行战，当执行栈中所有任务都执行完毕后，检查为任务队列是否有事件存在。无限重复此过程，就形成了循环，这个循环就叫做事件循环。

微任务包括但不限于以下几种：

+ Promise.then
+ MutationObserver
+ Object.observe
+ process.nextTick

宏任务包括但不限于以下几种：

+ setTimeout
+ setInterval
+ setImmediate
+ MessageChannel
+ requestAnimationFrame
+ I/O
+ UI交互事件

### 什么是执行栈？

当我们执行一个方法时，js会生成一个与该方法对于的执行环境（context）,又叫执行上下文。这个执行环境有这个方法的私有作用域，上层作用域的指向，方法的参数，私有作用域中定义的变量和this指向。这个执行环境会被添加到一个执行栈中，这个栈就是执行栈。

如果这个方法的代码又执行到了一行函数的调用语句，那么js会生成这个函数的执行环境并将其添加到执行栈中，然后进入这个执行环境继续执行其中代码，执行完毕并返回结果后，js会退出执行环境并把这个执行环境从栈中销毁，回到上一个方法的执行栈。

$nextTick，下次dom更新渲染后执行延迟回调，其实是下次微任务执行时更新dom，而$nextTick只是将回调添加到微任务中。

vue中更新dom的回调也是使用$nextTick注册的回调，都是向微任务队列中添加任务。