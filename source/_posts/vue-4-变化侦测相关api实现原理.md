---
title: vue-4.变化侦测相关api实现原理
tags:
  - 前端
categories:
  - - 笔记
    - vue
date: 2021-04-20 14:29:02
---


## vue-4.变化侦测相关api实现原理

### 4.1 vm.$watch

#### 4.1.1 用法

vm.$watch(expOrFn, callback, [options]);
参数：

+ {string | Function} expOrFn
+ {Function | Object } callback
+ {object} [options]
  + {boolean} deep
  + {boolean} immediate

+ 返回值：{Function} unwatch
+ 用法：用于观察一个表达式或computed函数在vue.js实例上的变化。回调函数调用时会得到新数据和旧数据。表达式只接受以点分隔的路径，例如a.b.c,如果是一个比较负责的表达式，可以用函数代替表达式。

例如：
vm.$watch('a.b.c', function(newVal,oldVal){});

返回一个取消观察函数，用来停止触发回调：
var unwatch = vm.$watch('a.b.c', function(newVal,oldVal){});
//取消观察
unwatch();

deep:true,发现对象内部值的变化
immediate:true,立即以表达式的当前值触发回调

#### 4.1.2 watch的内部原理

vm.$watch其实是对Watcher的一种封装，加上参数deep和immediate

```js
Vue.prototype.$watch = function (expOrFn, cb, options){
  const vm = this;
  options = options || {};
  const watcher = new Watcher(vm,expOrFn, cb, options);
  if(options.immediate){
    cb.call(vm,watcher.value);
  }
  return function unwatch(){
    watcher.teardown();
  }
}
```

先执行new Watcher实现vm.$watch的基本功能。
expOrFn是支持函数的，需要对Watcher进行简单的修改：

```js
export default calss Watcher{
  constructor(vm, expOrFn, cb){
    this.vm = vm;
    //expOrFn函数处理
    if(typeof expOrFn === 'function'){
      this.getter = expOrFn;
    }else{
      this.getter = parsePath(expOrFn)
    }
    this.cb = cb;
    this.value = this.get();
  }
}
```

新增判断expOrFn类型：如果是函数则直接赋值给getter；如果不是函数，使用parsePath函数来读取keypath中数据。keypath指的是属性路径，例如a.b.c,从vm.a.b.c读取数据。

当expOrFn是函数时，它不只可以动态返回数据，其中读取的所有数据都会被Watcher观察。当expOrFn只是keypath时，Watcher只会读取keypath所指向的数据并观察这个数据的变化。

执行new Watcher后判断是否使用immediate参数，如果使用立即执行一次cb

最后返回一个函数unwatch，取消观察数据。实际上执行watcher.teardown()来取消数据，实质上把watcher实例从正在观察的状态的依赖列表中移除。

首先需要在watcher中记录自己都订阅来谁，也就是watcher实例被收集进来哪些dep，然后当watcher不想继续订阅dep时，循环自己记录的订阅列表来通知他们Dep将自己从他们的依赖列表中移除。

```js
export default calss Watcher{
  constructor(vm, expOrFn, cb){
    this.vm = vm;
    this.deps = [];//新增
    this.depIds = new Set(); //新增
    //expOrFn函数处理
    if(typeof expOrFn === 'function'){
      this.getter = expOrFn;
    }else{
      this.getter = parsePath(expOrFn)
    }
    this.cb = cb;
    this.value = this.get();
  }
  // 新增：记录watcher中记录自己都订阅过哪些Dep
  addDep(dep){
    const id = dep.id;
    if(!this.depIds.has(id)){
      this.depIds.add(id);
      this.deps.push(dep);
      dep.addSub(this); //将自己订阅到Dep
    }
  }
}
```

只有第一次触发getter时候才会收集依赖。

在Watcher中新增addDep方法后，Dep中收集依赖的逻辑也需要改变：

```js
let uid = 0;
export default class Dep{
  constructor(){
    thid.id = uid++; //新增
    this.subs = [];
  }
  depend(){
    if(window.target){
      // this.addSub(window.target); //废弃
      window.target.addDep(this); //新增
    }
  }
}
```

此时，Dep会记录数据发送变化时，需要通知哪些Watcher，而Watcher中也同样记录了自己会被哪些Dep通知。他们是多对多的关系。

在watcher中新增teardown方法来通知订阅的Dep把自己从依赖列表中移除：

```js
teardown(){
  let i = this.deps.length;
  while(i--){
    this.deps[i].removeSub(this);
  }
}
```

执行Dep的removeSub方法将watcher从依赖列表中移除。

```js
export default class Dep{
  ...
  removeSub(sub){
    const index = this.subs.indexOf(sub);
    if(index > -1){
      return this.subs.splice(index,1)
    }
  }
}
```

#### 4.1.3 deep参数的实现原理

deep就是除了要触发当前这个被监听数据的收集依赖逻辑以外还要把当前监听的这个值内的所以子值都触发一遍收集依赖逻辑。

```js
export default calss Watcher{
  constructor(vm, expOrFn, cb){
    this.vm = vm;
    //新增
    if(options){
      this.deep = !!options.deep
    }else{
      this.deep = false
    }

    this.deps = [];
    this.depIds = new Set(); 
    //expOrFn函数处理
    if(typeof expOrFn === 'function'){
      this.getter = expOrFn;
    }else{
      this.getter = parsePath(expOrFn)
    }
    this.cb = cb;
    this.value = this.get();
  }

  get(){
    window.target = this;
    let value = this.getter.call(vm,vm);
    //新增 
    if(this.deep){
      traverse(value)
    }
    window.target = undefined;
    return value;
  }
  
}
```

如果使用deep参数在window.target = undefined之前调用traverse来处理deep逻辑。

递归value的所有子值来触发他们的收集依赖功能：

```js
const seenObjects = new Set();

export function traverse(val){
  _traverse_(val,seenObjects);
  seenObjects.clear();
}
function _traverse_(val,seen){
  let i,keys;
  const isA = Array.isArray(val);
  if((!isA && !isObject(val)) || Object.isFrozen(val)){
    return
  }
  if(val.__ob__){
    const depId = val.__ob__.dep.id;
    if(seen.has(depId)){
      return
    }
    seen.add(depId)
  }
  if(isA){
    i = val.length;
    while(i--) _traverse(val[i], seen)
  }else{
    keys = Object.keys(val);
    i = keys.length
    while(i--) _traverse(val[keys[i]], seen)
  }
}
```

这里我们先判断val的类型,如果它不是Array和 Object,或者已经被冻结,那么直接返回,什么都不干。

然后拿到val的dep.id,用这个id来保证不会重复收集依赖
如果是数组,则循环数组,将数组中的每一项递归调用 _traverse。
最后,重点来了,如果是 Object类型的数据,则循环 Object中的所有key,然后执行一次读取操作,再递归子值
while (i--) _traverse(val[keys[il], seen)
其中val[keys[i]]会触发 getter,也就是说会触发收集依赖的操作,这时 window.target还没有被清空,会将当前的 Watcher收集进去。
而 _traverse函数其实是一个递归操作,所以这个 value的子值也会触发同样的逻辑,这
羊就可以实现通过deep参数来监听所有子值的变化。