---
title: Array.from()的使用
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2023-06-02 09:36:01
---

Array.from()静态方法从可迭代或类数组对象创建一个新的浅拷贝的数组实例。

```js
console.log(Array.from('foo'))
// ['f', 'o', 'o']

console.log(Array.from([1, 2, 3], x => x + x))
// [2, 4, 6]
```

## 语法

```js
Array.from(arrayLike)
Array.from(arrayLike, mapFn?)
Array.from(arrayLike, mapFn?, thisArg?)
```

### 参数

`arrayLike`
想要转换成数组的可迭代或类数组对象

`mapFn`(可选)
调用数组每个元素的函数。如果提供，每个添加到数组的值首先传递给该函数，然后将mapFn的返回值添加到数组中。该函数提供以下参数：

  element
  数组当前正在处理的元素

  index
  数组当前正在处理的元素的索引

`thisArg`(可选)
执行mapFn时用作this的值

### 返回值

一个新的数组实例

### 描述

+ 可迭代对象（例如map和set）
+ 类数组对象（带有length属性和索引元素的对象）

Array.from不会创建{% post_link 稀疏数组 '稀疏数组' %},如果arrayLike缺少一些索引属性，那么这些属性在数组中将是undefined。

```js
console.log(Array.from({length: 5}))
// [undefined, undefined, undefined, undefined, undefined]
```

## 示例

### 从字符串构建数组

```js
Array.from("foo");
// [ "f", "o", "o" ]
```

### 从Set构建数组

```js
const set = new Set(["foo", "bar", "baz", "foo"]);
Array.from(set);
// [ "foo", "bar", "baz" ]
```

### 从Map构建数组

```js
const map = new Map([
  [1, 2],
  [2, 4],
  [4, 8],
]);
Array.from(map);
// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([
  ["1", "a"],
  ["2", "b"],
]);
Array.from(mapper.values());
// ['a', 'b'];

Array.from(mapper.keys());
// ['1', '2'];
```

### 从NodeList创建数组

```js
const images = document.querySelectorAll("img");
console.log(images)
const sources = Array.from(images, (image) => image.src);
console.log(sources)
const insecureSources = sources.filter((link) => link.startsWith("http://"));
console.log(insecureSources)

// output:
// NodeList(14) [img, img, img, img, img, img, img, img, img, img, img, img, img, img]

// (14)['http://localhost:8080/static/image/manage/unfold.svg', 'http://localhost:8080/static/image/workbench/workInstructions.svg', 'http://localhost:8080/static/image/workbench/day.svg', 'http://localhost:8080/static/image/workbench/briefing.svg', 'http://localhost:8080/static/image/workbench/feedback.svg', 'http://localhost:8080/static/image/workbench/schedule.svg', 'http://localhost:8080/static/image/workbench/task.svg', 'http://localhost:8080/static/image/workbench/achievement.svg', 'http://localhost:8080/static/image/workbench/waitclock.svg', 'http://localhost:8080/static/image/tabber/workbench-on.png', 'http://localhost:8080/static/image/tabber/ranking-list.png', 'http://localhost:8080/static/image/tabber/punch.png', 'http://localhost:8080/static/image/tabber/handle.png', 'http://localhost:8080/static/image/tabber/wode.png']

// (14)['http://localhost:8080/static/image/manage/unfold.svg', 'http://localhost:8080/static/image/workbench/workInstructions.svg', 'http://localhost:8080/static/image/workbench/day.svg', 'http://localhost:8080/static/image/workbench/briefing.svg', 'http://localhost:8080/static/image/workbench/feedback.svg', 'http://localhost:8080/static/image/workbench/schedule.svg', 'http://localhost:8080/static/image/workbench/task.svg', 'http://localhost:8080/static/image/workbench/achievement.svg', 'http://localhost:8080/static/image/workbench/waitclock.svg', 'http://localhost:8080/static/image/tabber/workbench-on.png', 'http://localhost:8080/static/image/tabber/ranking-list.png', 'http://localhost:8080/static/image/tabber/punch.png', 'http://localhost:8080/static/image/tabber/handle.png', 'http://localhost:8080/static/image/tabber/wode.png']
```

### 从类数组构建数组（arguments）

```js
function test() {
    return Array.from(arguments)
}
test(1,2,3)
// [1,2,3]
```

### 使用箭头函数和 Array.from()

```js
Array.from([1,2,3], (x) => x + x)
// [2,4,6]
Array.from({length: 5}, (v, i) => i)
// [0, 1, 2, 3, 4]
```

### 序列生成器range

```js
// 序列生成器函数（通常称为“range”，例如 Clojure、PHP 等）
const range = (start, stop, step) =>
  Array.from({ length: (stop - start) / step + 1 }, (_, i) => start + i * step);

// 生成的数字范围 0..4
range(0, 4, 1);
// [0, 1, 2, 3, 4]

// 生成的数字范围 1..10，步长为 2
range(1, 10, 2);
// [1, 3, 5, 7, 9]

// 使用 Array.from 生成字母表，并将其序列排序
range("A".charCodeAt(0), "Z".charCodeAt(0), 1).map((x) =>
  String.fromCharCode(x),
);
// ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"]
```

### 在非数组构造函数上调用 from()

from() 方法可以在任何构造函数上调用，只要该构造函数接受一个表示新数组长度的单个参数。

```js
function NotArray(len) {
  console.log("NotArray called with length", len);
}

// 可迭代对象
console.log(Array.from.call(NotArray, new Set(["foo", "bar", "baz"])));
// NotArray called with length undefined
// NotArray { '0': 'foo', '1': 'bar', '2': 'baz', length: 3 }

// 类数组对象
console.log(Array.from.call(NotArray, { length: 1, 0: "foo" }));
// NotArray called with length 1
// NotArray { '0': 'foo', length: 1 }
```

当 this 值不是构造函数，返回一个普通的数组对象。

```js
console.log(Array.from.call({}, { length: 1, 0: "foo" })); // [ 'foo' ]
```
