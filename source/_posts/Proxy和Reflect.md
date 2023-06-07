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

