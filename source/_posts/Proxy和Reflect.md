---
title: Proxy和Reflect
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2023-06-07 17:35:29
---

## Proxy

Proxy对象用于创建对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

语法：

```js
const p = new Proxy(target, handler)
```

参数：

`target`
要使用Proxy包装的对象(可以是任何类型的对象，包括数组，函数，甚至另一个代理)

`handler`
一个通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理 p 的行为。

### handler对象的方法

handler 对象是一个容纳一批特定属性的占位符对象。它包含有 Proxy 的各个捕获器（trap）。

traps:提供属性访问的方法

所有的捕捉器是可选的。如果没有定义某个捕捉器，那么就会保留源对象的默认行为。

`handler.getPropertyOf()`
Object.getPropertyOf 方法的捕获器

`handler.setPropertyOf()`
Object.setPropertyOf 方法的捕获器

`handler.isExtensible()`
Object.isExtensible 方法的捕捉器

`handler.preventExtensions()`
Object.preventExtensions 方法的捕捉器

`handler.getOwnPropertyDescriptor()`
Object.getOwnPropertyDescriptor 方法的捕捉器

`handler.defindProperty()`
Object.defindProperty 方法的捕捉器

`handler.has()`
in 操作符的捕捉器

`handler.get()`
属性读取操作的捕捉器

`handler.set()`
属性设置操作的捕捉器

`handler.deleteProperty()`
delete 操作符的捕捉器

`handler.ownKeys()`
Object.getOwnPropertyNames方法和Object.getOwnPropertySymbols方法的捕获器

`handler.apply()`
函数调用操作符的捕获器

`handler.constructor()`
new 操作符的捕获器

### 示例

当对象中不存在属性名时，默认返回值为 37

```js
const handler = {
    get: function(obj, prop) {
        return prop in obj ? obj[prop] : 37;
    }
};

const p = new Proxy({}, handler);
p.a = 1;
p.b = undefined;

console.log(p.a, p.b);      // 1, undefined
console.log('c' in p, p.c); // false, 37
```

无操作转发代理

```js
let target = {};
let p = new Proxy(target, {});

p.a = 37;   // 操作转发到目标

console.log(target.a);    // 37. 操作已经被正确地转发
```

通过代理，你可以轻松地验证向一个对象的传.set

```js
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // The default behavior to store the value
    obj[prop] = value;

    // 表示成功
    return true;
  }
};

let person = new Proxy({}, validator);

person.age = 100;

console.log(person.age);
// 100

person.age = 'young';
// 抛出异常：Uncaught TypeError: The age is not an integer

person.age = 300;
// 抛出异常：Uncaught RangeError: The age seems invalid
```


vue采用什么数据劫持？
vue2： Object.defineProperty
vue3: Proxy

为什么取代？
1.对象直接添加新的属性或删除已有的属性，界面不会自动更新，不是响应式
2.直接通过下标arr[1] = "xx"更改元素或数组长度，界面不会自动更新。
vue2的响应式的核心是通过defineProperty对对象已有的属性值的读取和修改进行劫持。
只能劫持对象的属性，我们需要对每个对象的每个属性进行遍历，无法监控到数组下标的变化。

通过Proxy代理：拦截对象本身的操作，如属性的添加、删除，值的读写；可以监听对象而非属性，监听数组的变化。


Proxy缺点：
Object.defineProperty: IE8以下不兼容
Proxy：IE9以下不兼容，Edge12+支持

