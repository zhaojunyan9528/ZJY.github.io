---
title: vue-3.Array的变化侦测
tags:
  - 前端
categories:
  - - 笔记
    - vue
date: 2021-04-20 12:38:37
---

## vue-3.Array的变化侦测

### 3.1 如何追踪变化？

使用自定义的方法覆盖原生的原型方法。
我们可以用一个拦截器覆盖Array.prototype.之后每当使用Array原型上的方法操作数组时，其实执行的都是拦截器中提供的方法，比如push方法。然后在拦截器中使用原生Array的原型方法去操作数组。

### 3.2 拦截器

拦截器其实就是一个和Array.prototype一样的object，里面包含的属性一摸一样，只不过这个object中某些可以改变数组自身内容的方法是我们处理过的。

Array原型中可以改变数组自身内容的方法有7个，分别是push,pop,unshift,shift,splice,sort,reverse.

```js
const arrayProto = Array.prototype
export const arrayMethods = Object.create(arrayProto);
[
'push',
'pop',
'unshift',
'shift',
'splice',
'sort',
'reverse'
].forEach(function (method){
const original = arrayProto[method]; //缓存原始方法
Object.defineProperty(arrayMethods, method, {
  value: function mutator(...args){
    return original.apply(this, args);
  },
  enumerable: false,
  writable: true,
  configurable: true
  })
})
```

变量arrayMethods继承自Array.prototype，具备其所有功能，在arrayMethods上使用Object.defineProperty方法将那些可以改变数组自身内容的方法进行封装。
所以当使用push方法，实际上使用的是arrayMethods.push，也就是函数mutator。因此我们就可以在mutator函数作一些其他的事，比如发送变化通知。

### 3.3 使用拦截器覆盖Array原型

有了拦截器之后，想要使他生效，需要去覆盖Array.prototype，但是又不能直接去覆盖，因为这样会污染全局Array。我们只希望拦截那些响应式数组的原型。将数据转换为响应式的，需要通过Observer，所以只需要在Observer中使用拦截器覆盖那些即将被转换成响应式Array类型的数据的原型就好了：

```js
export class Observer{
  constructor(value){
    this.value = value;
    if(Array.isArray(value)){
      value.__proto__ = arrayMethods;
    }else{
      this.walk(value);
    }
  }
}
```

### 3.4 将拦截器方法挂载到数组的属性上

因为不是所以浏览器都支持__proto__属性，因此，如果不能使用__proto__属性，就直接将arrayMethods身上的这些方法设置到被侦测的数组上：

```js
import { arrayMethods } from './array'
//__proto__是否可用
const hasProto = '__proto__' in {};
const arrayKeys = Object.getOwnPropertyNames(arrayMethods);

export class Observer{
  constructor(value){
  this.value = value;
  if(Array.isArray(value)){
    //修改
    const augment = hasProto ? protoAugment : copyAugment;
    augment(value, arrayMethods, arrayKeys );
  }else{
    this.walk(value);
  }

  }
}
function protoAugment(target, src, keys){
  target.__proto__ = src
}
function copyAugment(target, src, keys){
  for(let i=0,l=keys.length; i<l; i++){
    const key = keys[i];
    def(target, key, src[key])
  }
}
```

使用hasProto判断浏览器是否支持__proto__：如果支持，则使用protoAugment函数来覆盖原型；如果不支持，则调用copyAugment函数将拦截器中的方法挂载到value上。

### 3.5 如何收集依赖？

list:[1,2,3,4,5]
不管value是什么，想要获取一个object某个属性的数据，要通过key来读取value，因此在读取list的时候，会触发这个名字叫做list的属性的getter，比如：this.list
Array的依赖和Object一样，也在defineReactive中收集：

```js
function defineReactive(data, key, val){
  if(typeof val === 'object'){
    new Observer(val)
  }
  let dep = new Dep();
  Object.definedProperty(data, key, {
    enumerable: true,
    configurable: true,
    get: function(){
      dep.depend();
      //这里收集Array的依赖
      return val;
    },
    set: function(newVal){
      if(val === newVal){
      return;
      }
      val = newVal;
      dep.notify();
    }
  })
}
```

所以，Array在getter中收集依赖，在拦截器中触发依赖

### 3.6 依赖列表存在哪儿？

vue.js把Array的依赖存放在Observer中：

```js
export class Observer{
  constructor(value){
    this.value = value;
    this.dep = new Dep(); //新增dep

    if(Array.isArray(value)){
      const augment = hasProto ? protoAugment : copyAugment;
      augment(value, arrayMethods, arrayKeys );
    }else{
      this.walk(value);
    }

  }
}
```

数组在getter中收集依赖，在拦截器中触发依赖，所以这个依赖保存位置很关键，必须在getter和拦截器中都可以访问到。
之所以将依赖保存在Observer，是因为在getter中可以访问到Observer实例，在Array拦截中可以访问到Observer实例。

### 3.7 收集依赖

```js
function defineReactive(data, key, val){
  let childOb = observe(val); //修改

  let dep = new Dep();
  Object.definedProperty(data, key, {
    enumerable: true,
    configurable: true,
    get: function(){
      dep.depend();
      //这里收集Array的依赖
      if(childOb){
        childOb.dep.depend();
      }
      return val;
    },
    set: function(newVal){
      if(val === newVal){
      return;
      }
      val = newVal;
      dep.notify();
    }
  })
  }
  //尝试为value创建一个Observer实例
  //如果创建成功直接然后新创建的实例
  //如果value已经存在一个observer实例则直接返回它
  export function observe(value, asRootData){
    if(!isObject(value)) {
      return
    }
    let ob;
    if(hasOwn(value, '__ob__') && value.__ob__ instanceof Observer){
      ob = value.__ob__;
    }else{
      ob = new Observer(value);
    }
    return ob
  }
```

### 3.8 在拦截器中获取Observer实例

因为Array拦截器是对原型的一种封装，所以可以在拦截器中访问到this。而dep保存在Observer中，所以需要在this上读到Observer的实例：

```js
function def(obj, key, val, enumerable){
  Object.defineProperty(obj, key, {
    value: val,
    enumerable: !!enumerable,
    writable: true,
    configurable: true
  })
}

export class Observer{
  constructor(value){
    this.value = value;
    this.dep = new Dep();
    def(value, '__ob__', this);//新增
    if(Array.isArray(value)){
      const augment = hasProto ? protoAugment : copyAugment;
      augment(value, arrayMethods, arrayKeys);
    }else{
      this.walk(value);
    }
  }
}
```

def函数在value上新增一个不可枚举的属性__ob__，这个属性就是当前Observer实例。这个属性既可以用来在拦截器中访问Observer实例，还可用来标记是否已被Observer转换成响应式数据。
当value被标记来__ob__后可以通过value.__ob__来访问observer实例，如果是Array拦截器，拦截器是原型方法，可以通过this.__ob__来访问Observer实例。

```js
[
'push',
'pop',
'unshift',
'shift',
'splice',
'sort',
'reverse'
].forEach(function (method){
const original = arrayProto[method]; //缓存原始方法
Object.defineProperty(arrayMethods, method, {
  value: function mutator(...args){
    const ob = this.__ob__; //新增
    return original.apply(this, args);
  },
  enumerable: false,
  writable: true,
  configurable: true
  })
})
```

在mutator函数中可以通过this.__ob__来获取Observer实例。

### 3.9 向数组的依赖发送通知

当侦测到数组变化时，需要向依赖发送通知，首先要能访问到依赖。前面已可以在拦截器中访问Observer实例，只需要在Observer实例中拿到dep属性就可以发送通知了：

```js
[
'push',
'pop',
'unshift',
'shift',
'splice',
'sort',
'reverse'
].forEach(function (method){
const original = arrayProto[method]; //缓存原始方法
Object.defineProperty(arrayMethods, method, {
  value: function mutator(...args){
    let result = original.apply(this, args);
    const ob = this.__ob__; 
    ob.dep.notify();//向依赖发送消息
    return result
  },
  enumerable: false,
  writable: true,
  configurable: true
  })
})
```

### 3.10 侦测数组元素的变化

前面说侦测数组的变化指的是数组自身的变化，比如是否新增一个元素，是否删除一个元素，其实数组中object上某个属性发送变化也需要发送通知。比如使用push新增一个元素，这个元素的变化也需要侦测。

在observer新增一些处理，让它可以将array也转换响应式的：

```js
export class Observer{
  constructor(value){
    this.value = value;
    def(value, '__ob__', this);

    //新增
    if(Array.isArray(value)){
      this.observerArray(value);
    }else{
      this.walk(value);
    }
  }
}
// 侦测Array中的每一项
function observerArray(items){
  for(let i = 0, l = items.length; i < l; i++){
    observe(items[i])
  }
}
```

observerArray方法作用是循环Array中的每一项，执行observe函数来侦测变化。
observe函数就是将数组的每个元素都执行一遍new Observer，是一个递归过程。

### 3.11 侦测新增元素的变化

数组中一些方法比如push可以新增内容，新增的内容也需要转换成响应式的来侦测变化，否则出现修改数组无法触发消息等问题。

只需要获取新增元素并使用Observer来侦测他们就行。

#### 3.11.1 获取新增元素

获取新增元素需要在拦截器中数组方法的类型进行判断。如果数组方法是push，unshift,splice（可以新增数组元素的方法），则把参数中新增的元素拿过来，用Observer侦测：

```js
[
'push',
'pop',
'unshift',
'shift',
'splice',
'sort',
'reverse'
].forEach(function (method){
const original = arrayProto[method]; //缓存原始方法

def(arrayMethods, method, function mutator(...args){
  const result = original.apply(this,args);
  const ob = this.__ob__;
  let inserted;
  switch(method){
    case 'push':
    case 'unshift':
      inserted = args;
      break;
    case 'splice':
      inserted = args.slice(2);
      break;
  }
  ob.dep.notify();
  return result;
})
```

通过swtich对method进行判断，将新增元素取处理，暂存在inserted中

#### 3.11.2 使用observer侦测新增元素

Observer会将自身实例附加到value的__ob__属性上，所以被侦测了变化的数据都有一个__ob__属性，数组元素也不例外。
因此可以在拦截器中访问到this.__ob__,然后调用__ob__上的observeArray方法就可以了：

```js
[
'push',
'pop',
'unshift',
'shift',
'splice',
'sort',
'reverse'
].forEach(function (method){
const original = arrayProto[method]; //缓存原始方法

def(arrayMethods, method, function mutator(...args){
  const result = original.apply(this,args);
  const ob = this.__ob__;
  let inserted;
  switch(method){
    case 'push':
    case 'unshift':
      inserted = args;
      break;
    case 'splice':
      inserted = args.slice(2);
      break;
  }
  if(inserted) ob.observeArray(inserted); //新增
  ob.dep.notify();
  return result;
})
```

### 3.12 关于Array的问题

Array的变化侦测是通过拦截原型的方式实现的。所以：
this.list[0] = 2;
this.list.length = 0;
以上2中修改数组不会触发重新渲染和watch。
vue.js的实现方法决定了无法对以上2中作拦截也就没办法响应。但可以通过:
this.list.splice(0,0,item);
vm.$set(list,0,item);
实现数据响应

### 3.13 总结

Array追踪变化的方式和Object不一样，他是通过创建拦截器去覆盖数组原型的方式来追踪变化。

为了不污染全局Array.prototype，在Observer中只针对需要侦测数据变化的数组使用__proto__来覆盖原型方法。但__proto__不是所有浏览器都支持它，针对不支持它的浏览，直接循环拦截器，把拦截器中方法设置到数组身上来拦截Array.prototype上的原生方法。

Array收集依赖方式和Object一样，都在getter中收集。但是因为数组要在拦截器中向依赖发送消息，所以把依赖保存在来Observer实例上。

在Observer中对每个侦测数据变化的数据加上__ob__标记，并把this保存在__ob__上，一方面为了标记数据已被侦测（防止重复侦测），另一方面可以通过__ob__拿到Observer实例，从而获取实例上的依赖，以便在拦截器中发送通知向依赖。

除了数组自身变化外，使用observeArray方法将数组每个元素都转换为响应式的并侦测变化。

除了侦测已有数据外，当新增元素时也需要进行变化侦测，根据数组方法提取新增元素，然后使用observeArray方法对新增元素进行变化侦测。

根据下标修改数组元素或者使用length清空数组操作无法拦截。

