---
title: vue-2.Object的变化侦测
tags:
  - 前端
categories:
  - - 笔记
    - vue
date: 2021-04-20 12:27:22
---

## vue-2.Object的变化侦测

### 2.1 什么是变化侦测？

从状态生成dom，再输出到用户界面显示的一整套流程叫渲染，应用在运行时会不停的进行重新渲染。
响应式系统的核心是变化侦测，侦测数据的变化，数据变化时，会通知视图进行响应的更新。

vue.js会自动通过状态生成dom，并将其输出到页面显示出来，这个过程叫做渲染。vue
.js的渲染是声明式的，通过模版来描述状态与dom之间的映射关系。
变化侦测分为两种类型：一种是“推”（push），一种是“拉”（pull）。

Angular和React中的变化都属于“拉”，这就是说当状态发生变化时，他不知道哪个状态变来，只知道状态可能变了，然后发送一个信号告诉框架，框架内部接收到信号后，会进行一个暴力对比来找出哪些dom节点需要重新渲染。在Angular中是脏检查的流程，在React中使用的是虚拟dom。

而Vue.js的变化侦测属于“推”。当状态变化时，vue.js就立刻知道了，且在一定程序上知道哪些状态变了。因此它知道的信息更多，就可以进行更细粒度的更新。

更细粒度的更新：如果一个状态绑定着好多个依赖，每个依赖表示一个具体的dom节点，那么当这个状态发送变化时，向这个状态的所有依赖发送通知，让他们进行dom更新操作。相比较而言，“拉”的粒度是最粗的。

但是有一定的代价，粒度越细，绑定的依赖就越多，依赖追踪在内存上的开销就越大。因此，从vue.js2.0开始它引入了虚拟dom，将粒度调整为中等粒度，即一个状态所绑定的依赖不再是dom节点，而是一个组件，这样状态变化后，会通知到组件，组件内部在使用虚拟dom进行对比。这可以大大降低依赖数量，从而降低依赖追踪所消耗的内存。

### 2.2如何追踪变化

如何侦测一个对象的变化？
使用Object.definedProperty和es6的Proxy

```js
function defineReactive(data, key, val){
  Object.defineProperty(data, key, {
    eumerable: true,
    configurable: true,
    get: function(){
      return val;
    },
    set: function(newVal){
      if(val === newVal){
        return;
      }
      val = newVal;
    }
  })
}
```

当从data中key读取数据时，get函数被触发；当往data的key中设置数据时，set函数被触发。

### 2.3 如何收集依赖？

在getter中收集依赖，在setter中触发依赖。

### 2.4 依赖收集到哪里？

每个key都有一个数组，用来存储当前key的依赖。假设依赖是一个函数，保存在window.target上：

```js
function defineReactive(data, key, val) {
    let dep = []; //新增
    Object.defineProperty(data, key, {
        eumerable: true,
        configurable: true,
        get: function () {
            dep.push(window.target); //新增
            return val;
        },
        set: function (newVal) {
            if (val === newVal) {
                return;
            }
            //新增
            for (let i = 0; i < dep.length; i++) {
                dep[i](newVal, val);
            }
            val = newVal;
        }
    })
}
```

新增数组dep，用来存储被收集的依赖，然后在set被触发时，循环dep以触发收集到的依赖。

将收集依赖的代码封装到一个Dep类，专门用来帮助我们管理依赖。使用这个类。可以收集依赖、删除依赖、通知依赖等。代码如下：

```js
export default class Dep {
    constructor() {
        this.subs = [];
    }

    addSub(sub) {
        this.subs.push(sub);
    }

    removeSub(sub) {
        remove(this.subs, sub);
    }

    depend() {
        if (window.target) {
            this.addSub(window.target);
        }
    }

    notify() {
        const subs = this.subs.slice();
        for (let i = 0, l = subs.length; i < l; i++) {
            subs[i].update();
        }
    }

    function remove(arr, item) {
        if (arr.length) {
            let index = arr.indexOf(item);
            if (index > -1) {
                return arr.splice(index, 1);
            }
        }
    }
}
```

再改造下definedReactive：

```js
function defineReactive(data, key, val) {
    let dep = new Dep(); //修改
    Object.defineProperty(data, key, {
        eumerable: true,
        configurable: true,
        get: function () {
            dep.depend(); //修改
            return val;
        },
        set: function (newVal) {
            if (val === newVal) {
                return;
            }
            val = newVal;
            dep.notify(); //新增
        }
    })
}
```

将依赖收集到Dep中。

### 2.5 依赖是谁？

在上面代码中，我们收集的依赖是window.target，那么它到底是谁呢？
当属性发送变化时，通知谁，就是收集的依赖。

我们要通知到用到数据的地方，这个用到数据的地方很多，而且类型还不一样，有可能是模版，也有可能是用户写的一个watch，这时需要抽象出一个集中处理这些情况的类。然后在收集依赖阶段只收集这个封装好的类的实例进来，通知也只通知它一个，接着，它负责通知其他地方。这个类就是Watcher,收集的就是watcher。

### 2.6 什么是Watcher？

Watcher是一个中介角色，当数据变化时通知它，它再通知其他地方。
关于Watcher，看一个经典的使用方式：
vm.$watch(‘data.b.c’, function (newVal, oldVal){});
当data.b.c属性发生变化时，触发第二个参数中的函数。
把这个watcher实例添加到data.b.c的属性依赖Dep中，当data.b.c的值发生变化时，通知watcher，接着，watcher再执行回调函数。
代码如下：

```js
export default class Watcher{
  constructor(vm, expOrFn, cb){
    this.vm = vm;
    //执行this.getter()可以读取data.b.c的内容
    this.getter = parsePath(expOrFn);
    this.cb = cb;
    this.value = this.get();
  }

  get(){
    window.target = this;
    let value = this.getter.call(this.vm, this.vm);
    window.target = undefined;
    return value;
  }

  update(){
    const oldValue = this.value;
    this.value = this.get();
    this.cb.call(this.vm, this.value, oldValue);
  }
}
```

在get方法中将this也就是watcher实例添加到window.target，然后读取data.b.c的值，会触发getter，然后触发依赖收集。这就导致将watcher实例赋给window.target然后再读取以下值触发getter就能将this添加到Dep依赖中。

依赖注入到Dep后，当data.b.c发生变化时，就会让依赖列表中的依赖循环触发update方法，也就是watcher中的update方法，而update方法会执行参数中的回调函数将value和oldValue传到参数中。

所以不管是用户执行vm.$watch还是模版中的data都是通过watcher来通知自己是否需要发生变化。

### 2.7 递归侦测所有的key

前面介绍代码只能侦测数据中的一个属性，如果希望将数据中的所以属性都侦测到，所以要封装一个Observer类，将数据内的所以属性都转换成getter/setter形式，然后去追踪他们的变化：

```js
export class Observer {
    constructor(value) {
        this.value = value;
        if (!Array.isArray(value)) {
            this.walk(value)
        }
    }

    //walk会将每一个属性都转换成getter/setter形式来追踪变化，只有数据类型为object时被调用
    walk(obj) {
        const keys = Object.keys(obj);
        for (let i = 0; i < keys.length; i++) {
            defineReactive(obj, keys[i], obj[keys[i]]);
        }
    }
}

function defineReactive(data, key, val) {
    if (typeof val === 'object') {
        new Observer(val)
    }
    let dep = new Dep();
    Object.definedProperty(data, key, {
        enumerable: true,
        configurable: true,
        get: function () {
            dep.depend();
            return val;
        },
        set: function (newVal) {
            if (val === newVal) {
                return;
            }
            val = newVal;
            dep.notify();
        }
    })
}
```

Observer类可以将一个object对象转换为可侦测的object（通过walk方法将属性转换getter/setter形式），在defineReactive中用new Observer(val)来递归子属性，这样可以把data中所以属性都转换为getter/setter形式来追踪变化。当data中属性发生变化时，就会通知对于依赖进行更新。

### 2.8 关于Object的问题

vue.js通过Object.definedProperty来将对象的key转换为getter/setter形式来追踪变化，但getter/setter只追踪一个数据是否被修改，无法追踪属性的新增和删除。为来解决这个问题，vue.js提供来2个api：vm.$set和vm.$delete。

### 2.9 总结

Object可以通过Object.definedProperty将属性转换成getter/setter的形式来追踪变化。读取数据时会触发getter，修改数据时会触发setter。

我们需要在getter中收集有哪些依赖发生了变化。当setter触发时，去通知getter中收集的依赖数据发生了变化。

收集依赖需要为依赖找一个存储的地方，即Dep，它用来收集依赖、删除依赖、通知依赖更新等。

所谓的依赖，其实就是Watcher，只有watcher触发的getter才会被收集，哪个watcher触发了getter，就把它收集到Dep中，当数据发生变化，就循环依赖列表通知所有watcher。

Watcher的原理是先把自己设置到全局唯一指定的地方（window.target），然后读取数据，触发getter，接着，getter中就会从全局唯一指定的地方去获取当前正在读取数据的watcher，并把这个watcher收集到Dep中，通过这样方式，Watcher可以主动去订阅任意一个数据的变化。

此外，Observer类把一个object中给的所有数据包括子数据都转换成响应式的。但是在对象上新增或删除属性都无法被追踪到。

如下图，Data、Observer、Dep、Watcher之间关系。

![image](/images/vue-data.png)

Data通过Observer转换成getter/setter形式来追踪变化。
当外界通过Watcher读取数据时，会触发getter从而将watcher添加到依赖中。
当数据发生变化，会触发setter，从而向Dep中的依赖（Watcher）发送通知。
Watcher接收到通知后，会向外界发送通知，变化通知到外界可能会触发视图更新，也有可能触发用户某个回调函数。