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

`handler.getPrototypeOf()`
Object.getPrototypeOf 方法的捕获器

`handler.setPrototypeOf()`
Object.setPrototypeOf 方法的捕获器

`handler.isExtensible()`
Object.isExtensible 方法的捕捉器

`handler.preventExtensions()`
Object.preventExtensions 方法的捕捉器

`handler.getOwnPropertyDescriptor()`
Object.getOwnPropertyDescriptor 方法的捕捉器

`handler.defineProperty()`
Object.defineProperty 方法的捕捉器

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

`handler.construct()`
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

## Reflect

Reflect是js内置的对象，提供拦截js操作的方法，和proxy handler方法相同。Reflect不是个函数对象，因此它是不可构造的。

### 描述

Reflect并不是一个构造函数，所以不能通过new运算符进行调用，也不能作为一个函数来调用。
Reflect的所有属性和方法都是静态的，像Math对象。

### 静态方法

Reflect 对象提供了以下静态方法，这些方法与 proxy handler 方法的命名相同。

`Reflect.defineProperty(target, propertyKey, attributes)`
和Object.defineProperty()类似，如果设置成功返回true

`Reflect.deleteProperty(target, propertyKey)`
作为函数的delete操作符，相当于执行delete target[name]

`Reflect.getOwnPropertyDescriptor(target, propertyKey)`
类似于 Object.getOwnPropertyDescriptor()。如果对象中存在该属性，则返回对应的属性描述符，否则返回 undefined。

`Reflect.getPrototypeOf(target)`
类似于Object.getPrototypeOf()

`Reflect.setPrototypeOf(target, prototype)`
设置对象原型的函数。返回一个 Boolean，如果更新成功，则返回 true

`Reflect.has(target, propertyKey)`
判断一个对象是否存在某个属性，和 in 运算符 的功能完全相同。

`Reflect.get(target, propertyKey[,receiver])`
获取对象身上某个属性的值，类似于target[name]

`Reflect.set(target, propertyKey, value[, receiver])`
将值分配给属性的函数。返回一个Boolean，如果更新成功，则返回true。

`Reflect.isExtensible(target)`
类似于 Object.isExtensible().

`Reflect.preventExtensions(target)`
类似于 Object.preventExtensions()。返回一个Boolean。

`Reflect.ownKeys(target)`
返回一个包含所有自身属性（不包含继承属性）的数组。(类似于 Object.keys(), 但不会受enumerable 影响).

`Reflect.apply(target, thisArugument, argumentsList)`
对一个函数进行调用操作，同时可以传入一个数组作为调用参数。和 Function.prototype.apply() 功能类似。

`Reflect.construct(target, argumentsList[, newTarget])`
对构造函数进行new操作，相当于执行，new target(...args)

### 示例

检查一个对象是否有特定属性：

```js
const duck = {
  name: 'Maurice',
  color: 'white',
  greeting: function() {
    console.log(`Quaaaack! My name is ${this.name}`);
  }
}

Reflect.has(duck, 'color');
// true
Reflect.has(duck, 'haircut');
// false
```

返回对象自身的属性：

```js
Reflect.ownKeys(duck);
// [ "name", "color", "greeting" ]
```

为这个对象添加一个新的属性：

```js
Reflect.set(duck, 'eyes', 'black');
// returns "true" if successful
// "duck" now contains the property "eyes: 'black'"
Reflect.ownKeys(duck)
// (5) ['name', 'color', 'greeting', 'eyes']
```

获取某个属性的描述：

```js
Reflect.getOwnPropertyDescriptor(duck, 'name')
// {value: 'Maurice', writable: true, enumerable: true, configurable: true}
```

## Object.getPrototypeOf()

Object.getPrototypeOf()静态方法返回指定对象的原型（即[[Prototype]]属性的值）

```js
const proto = {};
const obj = Object.create(proto);
Object.getPrototypeOf(obj) === proto; // true
```

## Object.setPrototypeOf()

Object.setPrototypeOf()静态方法可以将一个指定的对象的原型（即内部的[[Propotype]]的值）设置成另一个对象或null值。

```js
const obj = {};
const parent = { foo: 'bar' };

console.log(obj.foo);
// Expected output: undefined

Object.setPrototypeOf(obj, parent);
// {
  // [[Prototype]]: {
  //   foo: 'bar',
  //   [[Prototype]]: Object
  // }
}

console.log(obj.foo);
// Expected output: "bar"
```

## Object.defineProperty()

Object.defineProperty()静态方法会在一个指定的对象新增一个属性或修改现有属性，并返回此对象。

```js
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});

object1.property1 = 77;
// Throws an error in strict mode

console.log(object1.property1);
// Expected output: 42
```

## Object.getOwnPropertyDescriptor()

Object.getOwnPropertyDescriptor()静态方法返回一个对象，该对象描述给定对象属性的配置（即直接存在于对象上而不在原型链上的属性）。

```js
const object1 = {
  property1: 42
};

const descriptor1 = Object.getOwnPropertyDescriptor(object1, 'property1');
// {value: 42, writable: true, enumerable: true, configurable: true}

console.log(descriptor1.configurable);
// Expected output: true

console.log(descriptor1.value);
// Expected output: 42
```

## Object.getOwnPropertyNames()

Object.getOwnPropertyNames()静态方法返回一个数组，其包含对象种所有自有属性（包括不可枚举属性，但不包含使用symbol作为key的属性）

```js
const target = Symbol('a')
const object1 = {
  a: 1,
  b: 2,
  c: 3,
  [target]: 1
};

console.log(Object.getOwnPropertyNames(object1));
// Expected output: Array ["a", "b", "c"]
```

## Object.getOwnPropertySymbols()

Object.getOwnPropertySymbols()静态方法返回一个包含给定对象自有Symbol属性的数组。

```js
const object1 = {};
const a = Symbol('a');
const b = Symbol.for('b');

object1[a] = 'localSymbol';
object1[b] = 'globalSymbol';

const objectSymbols = Object.getOwnPropertySymbols(object1);
//  [Symbol(a), Symbol(b)]
```

## Object.isExtensible()

Object.isExtensible()静态方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。

```js
const object1 = {}
console.log(Object.isExtensible(object1)) // true

Object.preventExtensions(object1)
console.log(Object.isExtensible(object1)) // false
```

默认情况下，对象是可扩展的：可以向它们添加新属性，并且它们的 [[Prototype]] 可以被重新赋值。可以使用 Object.preventExtensions()、Object.seal()、Object.freeze() 或 Reflect.preventExtensions() 中的任一方法将对象标记为不可扩展。

```js
// 新对象是可拓展的。
const empty = {};
Object.isExtensible(empty); // true

// 它们可以变为不可拓展的
Object.preventExtensions(empty);
console.log(Object.isExtensible(empty)); // false

// 根据定义，密封对象是不可拓展的。
const sealed = Object.seal({});
console.log(Object.isExtensible(sealed)); // false

// 根据定义，冻结对象同样也是不可拓展的。
const frozen = Object.freeze({});
console.log(Object.isExtensible(frozen)); // false
```

## Object.preventExtensions()

Object.preventExtensions()静态方法可以防止新属性被添加（即阻止对象被扩展）。它还可以阻止对象的原型被重新指定。

```js
const object1 = {};

Object.preventExtensions(object1);

try {
  Object.defineProperty(object1, 'property1', {
    value: 42
  });
} catch (e) {
  console.log(e);
  // Expected output: TypeError: Cannot define property property1, object is not extensible
}
```

## Object.keys()

Object.keys()静态方法返回一个给定对象的自身可枚举字符串属性名组成的数组(不包含Symbol)

```js
const d= Symbol('d')
const object1 = {
  a: 'somestring',
  b: 42,
  c: false,
  [d]: 'test'
};

console.log(Object.keys(object1));
// Expected output: Array ["a", "b", "c"]
```

## Object.values()

Object.values()静态方法返回一个给定对象的自身可枚举字符串键属性的值组成的数组(不包含Symbol)。

```js
const d= Symbol('d')
const object1 = {
  a: 'somestring',
  b: 42,
  c: false,
  [d]: 'test'
};

console.log(Object.values(object1));
// Expected output: Array ['somestring', 42, false]
```

## Object.entries()

Object.entries()静态方法返回一个数组，包含对象自身可枚举字符串键属性的键值对。

```js
const object1 = {
  a: 'somestring',
  b: 42
};
console.log(Object.entries(object1))
// [
//   ['a', 'somestring']
//   ['b', 42]
// ]
```

## Object.assign()

Object.assign()静态方法将一个或多个源对象所有自身可枚举属性（包含Symbol）复制到目标对象中，并返回修改后的目标对象。目标对象也将被修改。

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const source1 = { b:3, d: 6}

const returnedTarget = Object.assign(target, source, source1);

console.log(target);
// Expected output: Object { a: 1, b: 3, c: 5, d: 6 }

console.log(returnedTarget === target); // true
```

如果目标对象与源对象具有相同的键（属性名），则目标对象中的属性将被源对象中的属性覆盖，后面的源对象的属性将类似地覆盖前面的源对象的同名属性。

Object.assign() 方法只会拷贝源对象可枚举的的自有属性到目标对象。该方法在源对象上使用 [[Get]]，在目标对象上使用 [[Set]]，因此它会调用 getter 和 setter。故它对属性进行赋值，而不仅仅是复制或定义新的属性。如果要合并的源对象包含 getter，这可能使其不适合将新属性合并到原型中.

字符串和 Symbol 类型属性都会被复制.

Object.assign()是浅拷贝，只复制属性值。

## Object.create()

Object.create()静态方法以一个现有对象为原型，创建一个新的对象。

```js
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // Inherited properties can be overwritten

me.printIntroduction();
// Expected output: "My name is Matthew. Am I human? true"

console.log(me)
// {
//   isHuman: true
//   name: "Matthew"
//   [[Prototype]]: Object
//     isHuman: false
//     printIntroduction: ƒ ()
//     [[Prototype]]: Object
// }

Object.getPrototypeOf(me) === person // true
```