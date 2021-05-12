---
title: ES6
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-05-08 10:14:27
---

### 1.let和const命令

1.let命令

<b>基本用法</b>
let用来声明变量，和var语法类似，但声明的变量只在let命令所在代码块中有效。

```js
for (let i = 0; i < 10; i++) {}

console.log(i);
//ReferenceError: i is not defined
```

<b>不存在变量提升</b>
var声明的变量会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined。
为了纠正这种现象，let命令改变了语法行为。

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

<b>暂时性死区</b>

```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

在代码块中，使用let命令声明变量之前，该变量是不可用的，在语法上称为“暂时性死区”（TDZ）。

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

```js
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

在let命令声明tmp变量之前，都属于变量tmp的死区。

“暂时性死区”意味着typeof不再是一个安全的操作。

```js
typeof x; // ReferenceError:x is not defined
let x;
```

如果一个变量没有被声明，使用typeof反而不会报错。

```js
typeof undeclared_variable // "undefined"
```

```js
// 不报错
var x = x;

// 报错
let x = x; // 使用let声明变量时，只要变量声明没有完成之前使用就会报错，在变量x声明语句还没有执行完成前就去取x的值，导致报错
// ReferenceError: x is not defined
```

总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

<b>不允许重复声明</b>
let不允许在同一个作用域，重复声明已被var和let声明过的变量

<b>块级作用域</b>
ES5 只有全局作用域和函数作用域，没有块级作用域

```js
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

<b>块级作用域与函数声明</b>
ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。

```js
// 情况一
if (true) {
  function f() {}
}

// 情况二
try {
  function f() {}
} catch(e) {
  // ...
}
```

但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

```js
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
```

在ES5中会得到'I am inside!'，因为在if内声明的函数f会被提升到函数头部，实际运行的代码如下

```js
// ES5 环境
function f() { console.log('I am outside!'); }

(function () {
  function f() { console.log('I am inside!'); }
  if (false) {
  }
  f();
}());
```

ES6规定浏览器的实现可以不遵守上面的规定：
+ 允许在块级作用域内声明函数。
+ 函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
+ 同时，函数声明还会提升到所在的块级作用域的头部

上面三条规则只对 ES6 的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当作let处理。
根据这三条规则，在浏览器的 ES6 环境中，块级作用域内声明的函数，行为类似于var声明的变量。

```js
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

上面代码在符合es6的浏览器中都会报错，实际运行下面代码：

```js
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

```js
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

ES6 的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错。
```js
// 不报错
'use strict';
if (true) {
  function f() {}
}

// 报错
'use strict';
if (true)
  function f() {}
```

2.const命令
<b>基本用法</b>
const声明一个只读的常量。一旦声明，常量的值就不能改变。
const声明的常量，也与let一样不可重复声明。

<b>本质</b>
const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了

<b>顶层对象的属性</b>
顶层对象，在浏览器环境指的是window对象，在Node指的是global对象。ES5之中，顶层对象的属性与全局变量是等价的。

```js
window.a = 1;
a //1

a = 2;
window.a //2
```

let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性

### 2.变量的解构赋值

<b>数组的解构赋值</b>
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构。

```js
let [a, b, c] = [1, 2, 3];
a //1
b //2
c //3
```

这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值
```js
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```

如果解构不成功，变量的值就等于undefined.
```js
let [foo] = []; //foo:undefined
let [bar, foo] = [1]; //foo:undefined
```

等号左边的模式，只匹配一部分的等号右边的数组，叫不完全解构，也可以解构成功

```js
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

如果等号的右边不是数组（或者严格地说，不是可遍历的结构），那么将会报错。

```js
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

对于 Set 结构，也可以使用数组的解构赋值。

```js
let [x, y, z] = new Set(['a', 'b', 'c']);
x // "a"
```

<b>默认值</b>
解构赋值允许指定默认值。

```js
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

ES6内部采用严格相等运算符===，判断一个位置是否有值，所以一个数组成员不严格等于undefined，默认值不会生效。

```js
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

默认值可以引用解构赋值的其他变量，但该变量必须已经声明。

```js
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError
```

<b>对象的解构赋值</b>
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```js
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```

如果变量名和属性名不一致，必须写出下面这样：

```js
var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```

对象的解构赋值是下面形式的简写:

```js
let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
```

对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

```js
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```

对象的解构赋值也可以指定默认值。默认值生效的条件是，对象的属性值严格等于undefined。

```js
var {x = 3} = {x: undefined};
x // 3

var {x = 3} = {x: null};
x // null
```

<b>字符串的解构赋值</b>
字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象

```js
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```

类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。

```js
let {length : len} = 'hello';
len // 5
```

<b>数值和布尔值的解构赋值</b>
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。






