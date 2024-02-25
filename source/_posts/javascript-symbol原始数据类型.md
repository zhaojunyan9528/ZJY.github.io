---
title: javascript-symbol原始数据类型
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-20 17:20:44
---

## Symbol

ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：

undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

Symbol 值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

```javascript
    let s = Symbol();

    typeof s
    // "symbol"
```

注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

Symbol函数可以接受一个字符串作为参数，表示对 Symbol实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

注意，Symbol函数的参数只是表示对当前 Symbol值的描述，因此相同参数的Symbol函数的返回值是不相等的。

```javascript
    // 没有参数的情况
    let s1 = Symbol();
    let s2 = Symbol();
    
    s1 === s2 // false
    
    // 有参数的情况
    let s1 = Symbol('foo');
    let s2 = Symbol('foo');
    
    s1 === s2 // false
```