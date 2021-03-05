---
title: JavaScript this 关键字
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-20 17:15:47
---

## JavaScript this 关键字

面向对象语言中this表示对对象的一个引用。

但在javascript中，this不是固定不变的，它随执行环境的改变而改变。

+ 在方法中，this表示该方法所属的对象。
+ 如果单独使用，this代表全局对象
+ 在函数中，this代表全局对象
+ 在函数中，严格模式下，this是未定义的undefined
+ 在事件中，this表示接收事件的元素
+ 类似call(),apply(),bind()可以将this引用到任何对象

实例：

```js
var person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
```

### 方法中的this

在对象方法中，this指向调用它所在方法的对象。

上面实例中，this指person对象。
fullName 方法所属的对象就是 person。

```js
person.fullName(); // John Doe
```

### 单独使用this

单独使用this，this指全局对象

在浏览器中，全局对象指window

```
console.log(this === window); //true
```

严格模式下，单独使用this，也是指全局对象。

```
'use strict'
console.log(this === window); //true
```

### 函数中使用this

在函数中，函数的所属者默认绑定到this
在浏览器中，this指全局对象

```js
function myFunction() {
  return this;
}
myFunction(); //window
```

### 严格模式下函数中使用this

严格模式下，函数是没有绑定到this的，this是undefined

```js
'use strict'
function myFunction() {
  return this;
}
myFunction(); //undefined
```

### 事件中的this

在html事件柄中，this指向了接收事件的html元素：

```html
<button onclick="this.style.display='none'">
点我后我就消失了
</button>
```

### 对象方法中绑定

下面实例中，this 是 person 对象，person 对象是函数的所有者：

```js
var person = {
  firstName  : "John",
  lastName   : "Doe",
  id         : 5566,
  myFunction : function() {
    return this;
  }
};
person.myFunction(); //{firstName: "John", lastName: "Doe", id: 5566, myFunction: ƒ}
```

```js
var person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
person.fullName(); //John Doe
```

 this.firstName 表示 this (person) 对象的 firstName 属性。

### 显式函数绑定

```js
var person1 = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}
var person2 = {
  firstName:"John",
  lastName: "Doe",
}
person1.fullName.call(person2);  // 返回 "John Doe"
```

在 JavaScript 中函数也是对象，对象则有方法，apply 和 call 就是函数对象的方法。这两个方法异常强大，他们允许切换函数执行的上下文环境（context），即 this 绑定的对象。