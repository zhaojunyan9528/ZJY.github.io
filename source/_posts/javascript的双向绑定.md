---
title: javascript的双向绑定
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-02-28 17:13:49
---

实现双向绑定方式：

+ 1.javascript的get、set方法
+ 2.Object.defineProperty
+ 3.es6的proxy

## JavaScript的get,set方法

在创建新对象初始化时定义一个getter
get语法将对象属性绑定到查询该属性时将调用的函数

```js
var obj = {
  _t:'zjy',
  get fn(){
    return this._t;
  },
  set fn(_x){
    this._t = _x
  }
}
console.log(obj.fn); //zjy
console.log(obj._t); //zjy
obj.fn = 'hahaha'
console.log(obj.fn); //hahaha
console.log(obj._t); //hahaha
```

使用delete操作符删除getter

```js
delete obj.fn; //true
obj.fn; //undefined
obj._t; //hahaha
```

## Object.defineProperty

使用defineProperty在现有对象上定义getter
要随时在现有对象上添加getter，使用Object.defineProperty

```js
var obj = { a: 1};
Object.defineProperty(obj, 'b', {
  get: function(){
    return this.a + 1;
  }
});
console.log(obj.a); //1
console.log(obj.b); //2
obj.a = 2;
console.log(obj.a); //2
console.log(obj.b); //3

obj.b = 4;
console.log(obj.b); //3
```

使用计算出的属性名

```js
var expr = 'foo';

var obj = {
  get [expr]() { return 'bar'; }
};

console.log(obj.foo); //bar
```

### get vs. defineProperty

1.当使用 get 关键字时，它和Object.defineProperty() 有类似的效果，在classes中使用时，二者有细微的差别。

2.当使用 get 关键字时，属性将被定义在实例的原型上，当使用Object.defineProperty()时，属性将被定义在实例自身上

```js
class Example {
    get hello() {
        return 'world';
    }
}

const exa = new Example();
console.log(exa.hello);//world
```

Object.getOwnPropertyDescriptor(obj,property)方法返回指定对象上一个自有属性对应的属性描述符

```js
Object.getOwnPropertyDescriptor(exa,'hello'); //undefined
Object.getOwnPropertyDescriptor(getPropertyOf(exa),'hello'); //{ configurable: true, enumerable: false, get:funciton, set:undefined}
```

```js
var obj = {
    a: 'aaa'
}
Object.getOwnPropertyDescriptor(obj,'a')
//{ configurable: true, enumerable: true, value: "aaa", writable: true}
```

语法：Object.getOwnPropertyDescriptor(obj,prop)
参数：

+ 1.obj:需要查找的对象
+ 2.prop:目标对象内属性名称
+ 3.返回值：如果指定的属性存在对象上，则返回其属性描述符对象。否则返回undefined
+ 4.描述：该方法允许对一个属性的描述进行检索。属性描述符由以下组成：
  + value：该属性的值（仅针对数据属性描述符有效）
  + writable：当且仅当该属性值可修改时为true（仅数据描述符有效）
  + get: 获取该属性的访问器函数，如果没有访问器，则该值为undefined（仅针对包含访问器或设置器的属性描述符有效）
  + set: 获取该属性的设置器函数，如果没有设置器，则该值为undefined（仅针对包含访问器或设置器的属性描述符有效）
  + configurable: 当且仅当对象属性描述可以被改变或者该属性可以被删除时，该值为true
  + enumerble: 当且仅当属性可枚举时，该值为true

访问器属性描述符

```js
var obj = {
  get foo(){
    return 17;
  }
};
console.log(Object.getOwnPropertyDescriptor(obj,'foo'));
//{ enumerable: true, configurable: true, get:f foo(), set:undefined}
```

数据属性描述符：

```js
var obj = {
  foo:'bar'
}
console.log(Object.getOwnPropertyDescriptor(obj,'foo'));
//{ writable: true, value:'bar', configurable: true, enumerable:true}
```

使用Object.defineProperty(obj,prop)

```js
var o = {}
Object.defineProperty(o,'foo',{
    value: 12,
    writable: false,
    configurable: false,
    enumerable:false
})
console.log(Object.getOwnPropertyDescriptor(o,'foo'));
// {value: 12, writable: false, enumerable: false, configurable: false}
```

Object.defineProperty()方法会直接在一个对象上定义一个新的属性，或者修改一个对象的现有属性，并返回此对象。
备注：应当直接在 Object 构造器对象上调用此方法，而不是在任意一个 Object 类型的实例上调用。

```js
const obj1 = {};
Object.defineProperty(obj1,'property1',{
    value:44,
    writable: false
});
obj1.property1 = 77;//// throws an error in strict mode
console.log(obj1.property1); //44
```

语法：Object.defineProperty(obj,prop,desc)
参数：

+ obj: 要定义属性的对象
+ prop: 要定义或要修改的属性名称或Symbol
+ desc：要定义或要修改的属性描述符

返回值：被传递给函数的对象
描述：该方法允许精确的添加或修改对象的属性。通过赋值操作添加的普通属性都是可枚举的（例：var obj = {a:'a'}）.可通过for...in或Object.keys枚举到。可以修改这些属性的值，也可以删除属性。

默认情况下，通过Object.defineProperty方法添加的属性是不可修改的。
对象里目前存在2种类型的描述符：

+ 数据描述符：具有值的属性，该值可被修改也可以不能被修改
+ 存取描述符：由getter或者setter函数所描述的属性

一个描述符只能是2种描述符的一种，不能同时存在。他们共享以下可选健值（默认值是指在使用Object.defineProperty()定义属性时的默认值）：

+ configurable: 当且仅当该值为true时，该属性的值可修改，属性也可以被删除
+ enumerable：当且仅当该值为true时，该属性可以被枚举

数据描述符还具有以下可选健值：

+ value：该属性对应的值，可以是任何任何js值，默认为undefined
+ writable: 当且仅当该值为true时，value的值才可被赋值运算修改，默认为false

存取描述符还具有以下可选健值：

+ get：属性的getter函数，如果没有，则为undefined，当访问该属性时，会调用该函数，执行时不传入任何参数，但是会默认传入this对象，由于继承关系，this不一定是该属性所属对象，该函数的返回值被用作属性的值默认为undefined
+ set：属性的setter函数，如果没有，则为undefined，当属性值被修改时，会调用该函数，该方法接受一个参数。默认把赋值时的this对象传入，默认值为undefined

描述符汇总：
拥有布尔值的属性：writable,configurable,enumerable,默认值false
属性值和函数的键：value,get,set，默认值undefined

描述符可以拥有的健值：

|   | enumerable | configurable | writable  | value | get | set |
| -- | -- | -- | -- | -- | -- | -- |
|数据描述符 | 可以 | 可以 | 可以 | 可以 | 不可以 | 不可以 |
|存取描述符 | 可以 | 可以 | 不可以 | 不可以 | 可以 | 可以 |

如果一个描述符不具有value,writable，get，set中任意一个键,那么它将会被认为是一个数据描述符.

如果一个描述符同时拥有value或writable和get或set值,则会产生异常.

这些选项不一定是自身属性,也要考虑继承来的属性.

### 创建属性：

如果对象中不存在指定的值,Object.defineProperty()会创建这个属性.当描述符中省略某些字段时,这些字段会使用默认值.

```js
var o = {};//创建一个新对象
//在对象中添加一个属性与属性描述符的示例
Object.defineProperty(o,"a",{
  value: 37,
  writable: true,
  enumerable: true,
  configuarable: true
});
console.log(o.a);
// 对象 o 拥有了属性 a，值为 37

//在对象中添加一个设置了存取描述符属性的示例
var bValue = 38;
Object.defineProperty(o,"b",{
  //使用了方法名称缩写（ES2015特性）
  //下面两个缩写等价于：
  //get:function() { return bValue},
  //set:function() { return bValue;},
  get() {return bValue;},
  set(newValue) { bValue = newValue},
  enumerable: true,
  configuarable: true
});
o.b; //38
console.log(o.b);
// 对象 o 拥有了属性 b，值为 38

// 数据描述符和存取描述符不能混合使用
// Object.defineProperty(o,"conflict", {
//   value: 0x9f91102,
//   get() { return 0xdeadbeef}
// })
```

### 修改属性：

如果属性已经存在，Object.defineProperty将尝试根据描述符中的值以及对象当前配置来修改这个属性。
如果旧属性将configurable值设置false，则该属性不可被配置，其他属性不可修改（除来单向将writable设置为false）
当试图改变不可配置属性的值时,除了value和writable属性之外,会抛出typeerror,除非当前值和新值相同.

### writable属性：

当writable属性设置为false时,该属性不可写,不能重新被赋值.

```js
var o = {};
Object.defineProperty(o,'a',{
    value: 37,
    writable: false
});
console.log(o.a); //37
o.a = 25;
console.log(o.a); //37


(function() {
  'use strict';
  var o = {};
  Object.defineProperty(o,'b',{
      value: 2,
      writable: false
  });
  // o.b = 3; //throws typeerror: "b" is read-only
  return o.b;
}());
```

### enumerable属性

enumerable属性定义了对象的属性是否可以被for...in或Object.keys所枚举

```js
var o = {};
Object.defineProperty(o, "a", { value : 1, enumerable: true });
Object.defineProperty(o, "b", { value : 2, enumerable: false });
Object.defineProperty(o, "c", { value : 3 }); // enumerable 默认为 false
o.d = 4; // 如果使用直接赋值的方式创建对象的属性，则 enumerable 为 true
Object.defineProperty(o, Symbol.for('e'), {
  value: 5,
  enumerable: true
});
Object.defineProperty(o, Symbol.for('f'), {
  value: 6,
  enumerable: false
});
for(var i in o){
    console.log(i);// a,d
}

console.log( Object.keys(o)); //["a","d"]
console.log(o.propertyIsEnumerable('a')); //true
console.log(o.propertyIsEnumerable('b'));//false
console.log(o.propertyIsEnumerable('c'));//false
console.log(o.propertyIsEnumerable('d'));//true
console.log(o.propertyIsEnumerable(Symbol.for('e')));//true
console.log(o.propertyIsEnumerable(Symbol.for('f')));//false
console.log(o);
var p = { ...o}
console.log(p.a);//1
console.log(p.b);//undefined
console.log(p.c);//undefined
console.log(p.d);//4
console.log(p[Symbol.for('e')]);//5
console.log(p[Symbol.for('f')]);//undefined
```

### configurable属性

configurable属性定义了该属性是否可被删除，以及除value和writable属性外属性是否可以被修改

```js
var o = {};
Object.defineProperty(o,'a',{
   get() { return 1;},
   configuarable: false
});
Object.defineProperty(o,'a',{
    configuarable: true
});//typeerror:Cannot redefine property 'a'
Object.defineProperty(o,'a', {
    enumerable: true
});//typeerror:Cannot redefine property 'a'
Object.defineProperty(o,'a',{
    set() {}
});//typeerror:Cannot redefine property 'a'
Object.defineProperty(o,'a',{
    get() {return 1;}
});//typeerror:Cannot redefine property 'a'
Object.defineProperty(o,'a',{
    value: 12
});//typeerror:Cannot redefine property 'a'
console.log(o.a); //1
delete o.a;//false Nothing happens
console.log(o.a);//1
```

如果 o.a 的 configurable 属性为 true，则不会抛出任何错误，并且，最后，该属性会被删除

添加多个属性和默认值：
考虑特性被赋予的默认特性值非常重要,通常,使用点运算符和Object.defineProperty()为对象的属性赋值时,
数据描述符中的属性默认值是不同的

```js
var o = {};
o.a = 1;
console.log(Object.getOwnPropertyDescriptor(o,'a'));
// a {
//     configurable: true
//     enumerable: true
//     value: 1
//     writable: true
// }
// 等同于:
Object.defineProperty(o,'a',{
   value:1,
   writable: true,
   configurable: true,
   enumerable: true
});
//另一方面：
Object.defineProperty(o,'b',{value:1});
console.log(Object.getOwnPropertyDescriptor(o,'b'));
// b{
//     configurable: false
//     enumerable: false
//     value: 1
//     writable: false
// }
// 等同于:
Object.defineProperty(o,'b', {
   value:1,
   configurable:false,
   enumerable: false,
   writable: false
})
```

### 自定义setters和getters：

下面的例子展示了如何实现一个自存档对象。当设置temperature 属性时，archive 数组会收到日志条目

```js
function Archiver() {
 var temperature = null;
 var archive = [];

 Object.defineProperty(this, 'temperature', {
   get: function() {
     console.log('get!');
     return temperature;
   },
   set: function(value) {
     temperature = value;
     archive.push({ val: temperature });
   }
 });

 this.getArchive = function() { return archive; };
}

var arc = new Archiver();
arc.temperature; // 'get!'
arc.temperature = 11;
arc.temperature = 13;
arc.getArchive(); // [{ val: 11 }, { val: 13 }]
```

下面这个例子中，getter 总是会返回一个相同的值。

```js
var pattern = {
 get: function () {
     return 'I alway return this string,whatever you have assigned';
 },
 set: function () {
     this.myname = 'this is my name string';
 }
};


function TestDefineSetAndGet() {
 Object.defineProperty(this, 'myproperty', pattern);
}


var instance = new TestDefineSetAndGet();
instance.myproperty = 'test';

// 'I alway return this string,whatever you have assigned'
console.log(instance.myproperty);
// 'this is my name string'
console.log(instance.myname);
```

### 继承属性

如果访问者的属性是被继承的，它的 get 和 set 方法会在子对象的属性被访问或者修改时被调用。
如果这些方法用一个变量存值，该值会被所有对象共享。

```js
function myclass() {}
var value ;
Object.defineProperty(myclass.prototype,"x",{
    get(){
        return value;
    },
    set(x) {
        value = x;
    }
})
var a = new myclass();
var b = new myclass();
a.x = 1;
console.log(b.x);//1
```

这可以通过将值存储在另一个属性中解决。在 get 和 set 方法中，this 指向某个被访问和修改属性的对象

```js
function myclass() {}
Object.defineProperty(myclass.prototype,"x",{
    get(){
        return this.stored_x;
    },
    set(x) {
        this.stored_x = x;
    }
})
var a = new myclass();
var b = new myclass();
a.x = 1;
console.log(a.x);//1
console.log(b.x);//undefined
```

不像访问者属性，值属性始终在对象自身上设置，而不是一个原型。然而，如果一个不可写的属性被继承，它仍然可以防止修改对象的属性。

```js
function myclass() {
}

myclass.prototype.x = 1;
Object.defineProperty(myclass.prototype, "y", {
 writable: false,
 value: 1
});

var a = new myclass();
a.x = 2;
console.log(a.x); // 2
console.log(myclass.prototype.x); // 1
a.y = 2; // Ignored, throws in strict mode 
console.log(a.y); // 1
console.log(myclass.prototype.y); // 1
```
