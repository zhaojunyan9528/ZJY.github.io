---
title: vue源码
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-18 17:24:08
---

## 1.变化侦测篇

vue最大特点是数据驱动视图。
UI = render(state)
状态state是输入，页面UI是输出，状态变化了，页面也随之变化。这种特性称之为数据驱动视图。

state、render(),UI，state和UI都是用户定的，不变的render()。所以vue就代表render(),当state变化后，经过一系列加工，最终变化反应在ui上。

变化侦测就是追踪状态，一旦数据发生变化就去更新视图。

数据驱动视图的关键点在于我们如何知道数据发生了变化，只要在数据变化时去通知视图即可。

### 1.使Object数据变得“可观测”

通过Object.defineProperty()方法我们可以知道数据在什么时候变化。

将对象的属性变为可观测：

```js
// 源码位置：src/core/observer/index.js

/**
 * Observer类会通过递归的方式把一个对象的所有属性都转化成可观测对象
 */
export class Observer {
  constructor (value) {
    this.value = value
    // 给value新增一个__ob__属性，值为该value的Observer实例
    // 相当于为value打上标记，表示它已经被转化成响应式了，避免重复操作
    defineReactive(value,'__ob__',this)
    if (Array.isArray(value)) {
      // 当value为数组时的逻辑
      // ...
    } else {
      this.walk(value)
    }
  }

  walk (obj: Object) {
    const keys = Object.keys(obj)
    for (let i = 0; i < keys.length; i++) {
      defineReactive(obj, keys[i])
    }
  }
}
/**
 * 使一个对象转化成可观测对象
 * @param { Object } obj 对象
 * @param { String } key 对象的key
 * @param { Any } val 对象的某个key的值
 */
function defineReactive (obj,key,val) {
  // 如果只传了obj和key，那么val = obj[key]
  if (arguments.length === 2) {
    val = obj[key]
  }
  if(typeof val === 'object'){
      new Observer(val)
  }
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get(){
      console.log(`${key}属性被读取了`);
      return val;
    },
    set(newVal){
      if(val === newVal){
          return
      }
      console.log(`${key}属性被修改了`);
      val = newVal;
    }
  })
}
```

### 2.依赖收集

让object数据变得可观测后，就知道数据什么时候发生变化，发生变化后，去通知视图。但是不能一个数据变化就通知整个视图更新。
视图里谁用到了这个数据就更新谁，把“谁用到了这个数据"称为“谁依赖了这个数据”，给每个数据都建立一个依赖数组，谁依赖了这个数据就把这个依赖放进这个依赖数组。当这个数据发生变化的时候，就把对应的依赖数组全都通知一遍。这个过程就是依赖收集。

什么时候就行依赖收集？什么时候通知依赖更新？

谁用到了数据，在数据变化时，就去通知谁。在获取数据时，可观测数据在获取时会触发getter属性，那么就可以在getter触发时收集这个依赖。当数据变化时会触发setter属性，在setter触发时去通知依赖更新。

总结：在getter中收集依赖，在setter中通知依赖更新。

知道了什么是依赖收集以及何时收集何时通知后，那么我们该把依赖收集到哪里？

为每一个数据建立一个依赖管理器，把这个数据所有依赖都管理起来：
```js
// 源码位置：src/core/observer/dep.js
export default class Dep{
    constructor(){
        this.subs = []
    }

    addSub(sub){
        this.subs.push(sub);
    }

    // 删除一个依赖
    removeSub(sub){
        remove(this.subs, sub);
    }
    // 添加一个依赖
    depend(){
        if (window.target) {
            this.addSub(window.target)
        }
    }
    // 通知依赖更新
    notify(){
        const subs = this.subs.slice();
        for(let i=0; i<subs.length; i++){
            subs[i].update();
        }
    }
}
/**
 * @description: 移除数组中某项
 * @param {*} arr：数组  item：要移除的项
 * @return {*}
 */
export function remove(arr,item){
    if(arr.length){
        let index = arr.indexOf(item);
        if(index > -1){
            return arr.splice(index,1)
        }
    }
}
```

有了依赖管理器后，就可以在getter中收集依赖，在setter中通知依赖更新了：
```js
function defineReactive(obj,key,val){
    if(arguments.length === 2){
        val = obj[key];
    }
    if(typeof val === 'object'){
        new Observer(val);
    }

    const dep = new Dep(); //实例化一个依赖管理器，生成一个依赖管理数组dep
    Object.defineProperty(obj,key, {
        enumerable: true,
        configurable: true,
        get(){
            dep.depend(); //在getter中收集依赖
            return val;
        },
        set(newVal){
            if(val === newVale){
                return;
            }
            val = newVal;
            dep.notify(); //在setter中通知依赖更新
        }
    })
}
```

### 3.依赖到底是谁

谁用到了数据，就就是依赖，就为谁创建一个watcher实例。之后当数据变化时，不去通知依赖更新，而是通知依赖对应的watcher实例，由watcher实例去通知真正的视图。

Watcher类的具体实现：

```js
export default class Watcher {
  constructor (vm,expOrFn,cb) {
    this.vm = vm;
    this.cb = cb;
    this.getter = parsePath(expOrFn)
    this.value = this.get()
  }
  get () {
    window.target = this;
    const vm = this.vm
    let value = this.getter.call(vm, vm)
    window.target = undefined;
    return value
  }
  update () {
    const oldValue = this.value
    this.value = this.get()
    this.cb.call(this.vm, this.value, oldValue)
  }
}

/**
 * Parse simple path.
 * 把一个形如'data.a.b.c'的字符串路径所表示的值，从真实的data对象中取出来
 * 例如：
 * data = {a:{b:{c:2}}}
 * parsePath('a.b.c')(data)  // 2
 */
const bailRE = /[^\w.$]/
export function parsePath (path) {
  if (bailRE.test(path)) {
    return
  }
  const segments = path.split('.')
  return function (obj) {
    for (let i = 0; i < segments.length; i++) {
      if (!obj) return
      obj = obj[segments[i]]
    }
    return obj
  }
}
```

Watcher先把自己设置到全局唯一的指定位置（window.target），然后读取数据。因为读取了数据，所以会触发这个数据的getter。接着，在getter中就会从全局唯一的那个位置读取当前正在读取数据的Watcher，并把这个watcher收集到Dep中去。收集好之后，当数据发生变化时，会向Dep中的每个Watcher发送通知。通过这样的方式，Watcher可以主动去订阅任意一个数据的变化

以上，就彻底完成了对Object数据的侦测，依赖收集，依赖的更新等所有操作。

### 4.不足之处

Object.defineProperty()只能检测到对象的取值和设置值，当我们向对象里添加新的key/value或删除已有的key/value时，无法观测到，导致添加或删除属性时，无法及时响应更新。
可以通过Vue.set和Vue.delete解决该问题。

### 5.总结

1.Data通过observer转换成了getter/setter的形式来追踪变化。
2.当外界通过watcher读取数据时，会触发getter从而将watcher添加到依赖中。
3.当数据发生来变化时，会触发setter，从而向Dep中的依赖（即watcher）发送通知。
4.watcher收到通知后，会向外界发送通知，通知到外界后可能会触发视图更新也肯能触发用户的某个回调函数等。

