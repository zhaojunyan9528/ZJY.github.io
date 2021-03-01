---
title: javascript的Object.entries方法
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 17:02:51
---

## Object.entries方法

Object.entries()方法会返回指定对象自身可枚举属性，并返回其健值对数组，其排列与使用与for...in遍历该对象时返回一致，只不过for...in还会枚举其原型链中属性。

```js
const object1 = {
    a: 'somestring',
    b: 42
};
console.log(Object.entries(object1));
// Array：[['a','somestring'],["b",42]]
for (const [key, value] of Object.entries(object1)) {
    console.log(`${key}: ${value}`);
}
//a: somestring
// b: 42
```

语法：Object.entries(obj)
参数：
obj: 可以返回枚举健值对的对象
返回值：返回指定对象可枚举属性的健值对数组

```js
const obj = {
  foo: 'bar',
  baz: 42
};
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// array like object
const obj1 = {
    0: 'a',
    1: 'b',
    2: 'c'
};
console.log(Object.entries(obj1)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]


// array like object with random key ordering
const anObj = {
    100: 'a',
    2: 'b',
    7: 'c'
};
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]
for (k in anObj) {
    console.log(k); //2   7   10
}


// getFoo is property which isn't enumerable
// create和defineProperty方法创建对象属性,enumerable默认值为false
const myObj = Object.create({}, {
  getFoo: {
      value() {
          return this.foo;
      }
  }
});
console.log(myObj); //{ getFoo:()}
myObj.foo = 'bar';
console.log(myObj); //{foo:"bar",getFoo:()}
console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]


// non-object argument will be coerced to an object
console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]



// iterate through key-value gracefully
const objs = {
    a: 5,
    b: 7,
    c: 9
};
for (const [key, value] of Object.entries(objs)) {
    console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
}

// Or, using array extras
Object.entries(objs).forEach(([key, value]) => {
    console.log(`${key} ${value}`); // "a 5", "b 7", "c 9"
});


// 将Object转换为Map:
// new Map() 构造函数接受一个可迭代的entries。借助Object.entries方法你可以
// 很容易的将Object转换为Map:
var obj3 = { foo: "bar", baz: 42 }; 
var map = new Map(Object.entries(obj3));
console.log(map); // Map { foo: "bar", baz: 42 }
```