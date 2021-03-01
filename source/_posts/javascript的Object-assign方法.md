---
title: javascript的Object.assign方法
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 15:20:49
---

Object.assign方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象，并返回目标对象

```js
const target = {
    a: 1,
    b: 2
};
const resourceObject = {
    b: 4,
    c: 5
};
const resourceObject1 = {
    d: 6
};
const returnObject = Object.assign(target, resourceObject, resourceObject1);
console.log(target);
console.log(returnObject);
// {
//   a:1,
//   b:4,
//   c:5,
//   d:6
// }
```

语法：Object.assign(target,...resource);
参数：

+ target：目标对象
+ resource：源对象
返回值：目标对象
描述：如果目标对象和源对象有相同属性，则目标对象属性值会被源对象覆盖，相应的后面的源对象的属性会覆盖前面源对象的属性

Object.assign只会拷贝源对象自身的并且可枚举属性到目标对象上。该方法使用源对象的getter方法和目标对象的setter方法。因此他分配属性不仅仅是复制或定义新的属性。
如果合并源包含getter，可能不适合将新属性合并到原型上，为了将属性定义复制到原型，应使用Object.defineProperty和Object.getOwnPropertyDescriptor()

注意，Object.assign不会在源对象属性值为null或undefined时抛出错误

```js
const a = {
    test:null
}
const b = {
    foo:undefined
}
Object.assign(a,b)
// {test:null, foo:undefined}
```

String和Symbol类型的属性都会被拷贝.

```js
const a = {
    a: 1,
    b: 2
};
const b = {
    c: Symbol.for(1),
    d: String('aaa')
};
Object.defineProperty(b, 'b', {
    writable: false,
    value: 4
});
const c = Object.assign(a, b);
console.log(c);
// object{
//     a:1,
//     b:2,
//     c:Symbol(1),
//     d:'aaa'
// }

// getter获取a.b属性的值，对目标对象b.b属性进行覆盖，调用b的setter方法，但b.b不可修改，报错
const c = Object.assign(b,a);//typeerror:cannot assign to read only property 'b' 
```

```js
const aa = {
    a: 'a'
};
const bb = {};
Object.defineProperty(bb, 'b', {
    get: function() {
        return 'i am b'
    }
});
const cc = Object.assign(aa, bb);
console.log(cc);
// object{
//     a:'a'
// }//不能将含有getter的属性合并到原型中
```

### 深拷贝问题:

针对深拷贝，需要使用其他办法，因为 Object.assign()拷贝的是（可枚举）属性值。
假如源值是一个对象的引用，它仅仅会复制其引用值。

```js
const log = console.log;
const stringify = JSON.stringify

function test() {
    'use strict';
    let obj1 = {
        a: 0,
        b: {
            c: 0
        }
    };
    let obj2 = Object.assign({}, obj1);
    log(stringify(obj2)); //{"a":0,"b":{"c":0}}

    obj1.a = 1;
    log(stringify(obj1)); //{"a":1,"b":{"c":0}}
    log(stringify(obj2)); //{"a":0,"b":{"c":0}}

    obj2.a = 2;
    log(stringify(obj1)); //{"a":1,"b":{"c":0}}
    log(stringify(obj2)); //{"a":2,"b":{"c":0}}

    obj2.b.c = 3;
    log(stringify(obj1)); //{"a":1,"b":{"c":3}}
    log(stringify(obj2)); //{"a":1,"b":{"c":3}}

    //deep clone
    obj1 = {
        a: 0,
        b: {
            c: 0
        }
    };
    let obj3 = JSON.parse(JSON.stringify(obj1));
    obj1.a = 4;
    obj1.b.c = 4;
    log(stringify(obj3)); //{"a":0,"b":{"c":0}}
}
test();
```

合并对象

```js
const o1 = {
    a: 1
};
const o2 = {
    b: 2
};
const o3 = {
    c: 3
};
const obj4 = Object.assign(o1, o2, o3);
console.log(obj4); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变

```

合并具有相同属性的对象

```js
const oo1 = {
  a: 1,
  b: 1,
  c: 1
};
const oo2 = {
  b: 2,
  c: 2
};
const oo3 = {
  c: 3
};

const obj5 = Object.assign({}, oo1, oo2, oo3);
console.log(obj5); // { a: 1, b: 2, c: 3 }
```

拷贝 symbol 类型的属性

```js
const v1 = {
    a: 1
};
const v2 = {
    [Symbol('foo')]: 2
};

const obj6 = Object.assign({}, v1, v2);
console.log(obj6); // { a : 1, Symbol("foo"): 2 }
console.log(Object.getOwnPropertySymbols(obj6)); // [Symbol(foo)]
```

继承属性和不可枚举属性是不能拷贝的

```js
const obj7 = Object.create({
    foo: 1
}, { // foo 是个继承属性。
    bar: {
        value: 2 // bar 是个不可枚举属性。
    },
    baz: {
        value: 3,
        enumerable: true // baz 是个自身可枚举属性。
    }
});

const copy = Object.assign({}, obj7);
console.log(copy); // { baz: 3 }
```

原始类型会被包装为对象

```js
const m1 = 'abc';
const m2 = true;
const m3 = 10;
const m4 = Symbol('foo');
const res = Object.assign({}, m1, null, m2, undefined, m3, m4);
console.log(res);
//{
//     0:'a',
//     1:'b',
//     2:'c',
// }
console.log(typeof res) //object
console.log(res instanceof String); //true

const res1 = Object.assign(true, 'abc');
console.log(res1);
// Boolean{
//     true,
//     0:'a',
//     1:'b',
//     2:'c'
// }
console.log(typeof res1); //object
console.log(res1 instanceof Boolean); //true


const res2 = Object.assign('abc', true);
console.log(res2);
// String{
//     0:'a',
//     1:'b',
//     2:'c'
// }
console.log(typeof res2); //object
console.log(res2 instanceof String); //true


const res3 = Object.assign(10, 'a');
console.log(res3);
// Number{10,0:'a'}
console.log(typeof res3); //object
console.log(res3 instanceof Number); //true
```

异常会打断后续拷贝任务

```js
const targetObj = Object.defineProperty({}, "foo", {
  value: 1,
  writable: false
}); // targetObj 的 foo 属性是个只读属性。
Object.assign(targetObj, {bar: 2}, {foo2: 3, foo: 3, foo3: 3}, {baz: 4});// TypeError: "foo" is read-only
```

注意这个异常是在拷贝第二个源对象的第二个属性时发生的。

```js
// console.log(targetObj);//{bar:2,foo2:3,foo:1}
// console.log(targetObj.bar);  // 2，说明第一个源对象拷贝成功了。
// console.log(targetObj.foo2); // 3，说明第二个源对象的第一个属性也拷贝成功了。
// console.log(targetObj.foo);  // 1，只读属性不能被覆盖，所以第二个源对象的第二个属性拷贝失败了。
// console.log(targetObj.foo3); // undefined，异常之后 assign 方法就退出了，第三个属性是不会被拷贝到的。
// console.log(targetObj.baz);  // undefined，第三个源对象更是不会被拷贝到的。
```

拷贝访问器

```js
const o = {
  foo: 1,
  get bar() {
      return 2;
  },
  [Symbol('foo')]: 1
};
let copy1 = Object.assign({}, o);
console.log('assign',copy1); //copy.bar的值来自obj.bar的getter函数的返回值 {foo:1,bar:2}
// assign{
//     bar:2,
//     foo:1,
//     Symbol(foo):1
// }
```