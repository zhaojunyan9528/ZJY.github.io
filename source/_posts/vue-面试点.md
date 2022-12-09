---
title: vue-面试点
tags:
  - 前端
categories:
  - - 面试
    - vue
date: 2021-01-20 11:37:13
---


### 1.mvvm

m,指数据模型（data）,v,指视图，UI组件，vm视图模型，把传统的mvc中controller演变成viewModel.
viewModel是view和model的桥梁，数据会绑定到viewModel并自动将数据渲染到页面，视图变化时会
通知viewModel层更新数据。

### 2.vue2中响应式原理

在初始化数据时，会使用Object.defineProperty()方法重新定义data中到所有属性，在使用对于属性时，
首先进行依赖收集（收集当前组件到watcher），如果属性发生变化时会通知相关依赖进行更新操作（订阅/发布）

### 3.vue3响应式原理

vue3使用proxy替代了object.defineproperty，proxy可直接检测到数组和对象到变化。并且有13中拦截方法。

### 4.vue3只会监测数组到第一层，vue3怎么解决？

判断Reflect.get到返回值类型是否为object，如果式用Reactive方法继续进行代理，实现数组到深度监测

### 5.监测数组可能多次触发get/set方法，如何防止？

判断key是否为当前被代理对象target自身属性，或者新值和旧值是否相等，两者满足其一可触发trigger

### 6.vue2如何监测数组变化？

使用了函数劫持，并重新了数组方法。vue对data中数组进行了原型链重写，并指向了自己定义的数组原型方法，
当调用数组api时，进行依赖更新，当数组中存在引用类型时，会对引用类型再次递归遍历进行监控属性监测数组。

### 7.nextTick

在下次Dom更新循环之后执行延迟回调。主要使用了宏任务和微任务，根据执行环境分别尝试采用：
promise/mutationobserve/setimmediate/settimeout定义了一个异步方法，多次调用nextTick会将方法存入队列中，
通过这个异步方法清空队列。

### 8.vue生命周期

+ beforeCreate:new Vue()后第一个钩子函数，在此阶段data/methods/computed/watch中方法和数据都不能被访问；
+ created:实例被创建之后，此时已完成数据观测，可以访问，更改数据，此时更改数据不回触发update函数，此时
可初始化数据，不可访问Dom，或者vm.$nextTick中进行访问
+ beforeMount:实例被挂载之前，template模版已导入渲染函数编译，此时虚拟dom已创建完成，即将开始渲染，可更新数据
+ mounted:实例被挂载之后，dom已挂载，数据完成双向绑定，可更新数据，通过ref访问真是dom
+ beforeUpdate:数据更新之前，响应式数据更新之前，虚拟dom重新渲染之前触发，可更新数据不触发update
+ updated:完成更新之后，dom已完成更新，避免此阶段更新数据，造成重复渲染
+ beforeDestory：实例销毁之前，实例扔可用，可做收尾清除工作
+ destoryed:实例被销毁后，实例不可用

### 9.接口请求放在哪个生命周期？

mounted和created中都可，根据是否需要访问dom决定，但是服务器渲染不支持mounted,需要放在created中

### 10.computed和watch区别？

computed本质上是一个具有缓存的watcher,依赖的属性发生变化就会更新视图，使用于消耗性能的场景
watch没有缓存，起到观察的作用，可以监听数据执行回调，深度监听，deep：true

### 11.v-if和v-show的区别？

v-if条件不成立时，不会渲染dom，v-show是display属性样式切换当前dom的显示和隐藏，
v-if有更高的切换消耗；v-show有更高的初始渲染消耗

### 12.组件中的data为什么是一个函数？

组件可以被多次复用，创建多个实例，这些实例本质是同一个构造函数，如果data是对象，对象是引用类型，
修改对象会影响所有实例，因此data是一个函数

### 13.v-model的原理

v-model是一个语法糖，是value+input的语法糖，根据model属性的prop和event属性来进行定义。原生的v-model会根据
标签的不同生成不同的事件和属性

### 14.vue事件绑定原理

原生绑定事件通过addEventlistener绑定给真实原素的，组件事件绑定通过vue自定义的$on实现的

### 15.vue模版编译的原理？

vue的编译过程就是将template转化为render函数的过程，经历以下阶段：

+ 将模版解析为AST（抽象语法树）
+ 遍历AST标记静态节点
+ 使用AST生成渲染函数

这三部分内容分别抽象出三个模块来实现各自的功能，分别是：

+ 解析器
+ 优化器
+ 代码生成器

首先解析模版，生成AST树，使用正则对模版进行解析，对不同标签文本用不同钩子进行处理
vue数据是响应式对，但不是所有数据都是响应式的，有一些数据渲染后就不会发生变化，其Dom也不会
发生变化，深度遍历AST树，标记这些静态节点就可以跳过比对
将优化过后的AST树转化为可执行的代码

解析器的作用是通过模版得到AST
生成AST需要借助html解析器，当html解析器触发不同的钩子函数时，我们可以构建出不同的节点。
html解析器的内部原理是一小段一小段的截取模版字符串，每截取一小段字符串就会根据截取处理的字符串类型触发不同的钩子函数，直到模版字符串截空停止运行。

优化器的作用是在AST找出静态子树并打上标记，这样作有2个好处：

+ 每次重新渲染时，不需要为静态子树创建新节点。
+ 在虚拟dom打补丁的过程可以跳过。

### 16.keep-alive

keep-alive可以实现组件缓存，当组件切换时不会对当前组件进行卸载
通过include/exclued实现组件有条件对进行缓存
两个生命周期activated/deactivated，用来判断当前组件是否处于活跃状态

### 17.vue组件生命周期调用顺序？

+ 组件调用顺序是先父后子，渲染完成顺序是先子后父
+ 组件销毁顺序是先父后子，销毁完成顺序是先子后父

加载渲染过程：
父beforeCreate->父created->父beforeMount-子beforecreate-子created-》子beforeMounted-》子mounted-》父mounted

子组件更新过程：
父beforeUpdated-》子beforeUpdated-》子updated-》父updated

父组件更新过程：
父beforeUpdated-》父updated

销毁过程：
父beforeDestory-》子beforeDestory-》子destoryed-》父destoryed

### 18.vue2组件通信

+ 父子间通信： 父-》子： props       子-》父：$on  $emit
获取父子组件实例 ：$parent  $children
ref获取实例的方式调用组件的属性或方法
+ 兄弟组件通信： Event Bus/vuex
+ 跨级组件通信：vuex  /Event Bus /$attrs  $listeners /Provide  inject

### 19.SSR

SSR服务端渲染，就是在客户端把标签渲染成html的工作放在服务端完成，然后将html返回给客户端
SSR有更好的SEO，首屏加载速度快，缺点是服务端渲染只支持beforeCreate和created两个钩子，需要nodejs环境，
对服务器有更大对负载需求

### 20.vue性能优化

+ 编码阶段：
    + 减少data中数据
    + v-if和v-show不能连用
    + SPA采用keep-alive缓存组件
    + 使用路由懒加载，异步组件
    + 第三模块按需引入
    + 图片懒加载
    + 防抖、节流

+ SEO优化：
    + 预渲染
    + 服务端渲染

+ 打包优化：
    + 压缩代码
    + tree shaking
    + cdn加载第三方模块
    + sourcemap优化

### 21.hash路由和history路由的实现原理

location.hash的值实际上就是url中#后面的值
history采用懒H5中提供的API来实现，主要有history.pushState() history.replaceState()

### 22.vuex

vuex和全局对象的区别：
vuex的状态存储是响应式的，组件从store中读取状态，当store中的状态变化时，会更新组件
不能直接改变store中状态，必须显式提交mutation

```javascript
const store = new Vuex.Store({
    state:{
        count:0
    },
    mutation:{
        increment(state){
            state.count++;
        }
    }
})
store.commit('increment');
console.log(store.state.count); //1
```

由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。触发变化也仅仅是在组件的 methods 中提交 mutation

### 23.什么是vuex

vuex是专门为vuejs应用程序开发的状态管理插件，集中存储管理组件状态，通过显式提交mutation改变组件状态

### 24.vuex解决了什么？

多个组件依赖同一个状态，多层组件间传值
来自不同的组件的行为需要变更同一个状态

怎么引用vuex？
1.安装依赖 npm install vuex --save
2.在项目目录创建store文件夹
3.在store文件夹下新建index。js文件，创建vuex实例
4.在main文件中引入vuex和store

vuex的5个核心属性？
state	mutation		getter		actions		modules

vuex的状态管理存储在state中，通过提交mutation改变状态

vuex中状态是对象时，使用时，要深度克隆赋值对象再修改，防止影响原始数据

vuex中action和mutation有什么区别？
action提交的是mutation，而不是直接变更状态，mutation可以直接变更状态
action是this.$store.dispatch()来提交，而mutation是this.$store.commit来提交
接收参数不同，mutation第一个参数是state，action是context

### 25.重定向页面：

routes:[{path:'/a', redirect: '/b' }]
配置404页面：

```javascript
{
    path: "/404",
    name: "notFound",
    component: notFound
}, 
{
    path: "*", // 此处需特别注意置于最底部
    redirect: "/404"
}
```

### 26.路由几种模式，区别是什么？

+ hash：兼容所有浏览器，但不支持H5 history api hash值为#后面的内容，通过监听hashChange事件
来完成操作实现前端路由，hash值变化不会请求服务器
+ history：依赖H5 history API实现前端路由，和正常url一样，但是初次请求或刷新会请求服务器，没有
请求到对于资源会返回404
+ abstract：支持所有js运行环境，如果发现没有浏览器到api路由会强制进入此模式

### 27.发布/订阅者模式：

vue内部实现了双向绑定机制，可以不再操作dom，此机制是通过数据劫持结合发布/订阅者模式实现的：通过object.defineProperty()来劫持各个属性的getter,setter属性，在数据变动时发布消息给订阅者，触发相应的监听回调。
订阅模式和观察者模式不一致，订阅模式有一个调度中心，对订阅事件统一进行管理。而观察者模式可以随意注册事件，调用事件。

### 28.数据双向绑定基础：Object.defineProperty()

+ 数据属性：

configurable—能否用delete删除属性从而重新定义属性
enumerable—能否通过for-in遍历，即是否可枚举
writable—能否修改属性的值，默认为true
value—属性的数据值，默认为undefined
若要修改上述4个数据属性，需要Object.defineProperty(属性所在对象，属性名，描述符对象)

```javascript
var person = {
    name:’’
}
Object.defineProperty(person,”name”,{
    writable: false,
    name:”test”
})
```

+ 访问器属性：

访问器属性不包含数据值value，包含一对getter,setter函数（非必须）。value和writable和get/set不能共存。
configurable—能否用delete删除属性从而重新定义属性
enumerable—能否通过for-in遍历，即是否可枚举
get—读取属性调用的函数，默认为undefined
value—读取属性调用的函数，默认为undefined

Object.create(null): this.set = Object.create(null)这样赋值，这样写不需要考虑原型链上的属性，可以真正创建一个纯净的对象。

### 29.es6

export default和export的区别：
1.在一个文件或模块中，可以有多个export，而export default只能有一个
2.通过export方式导出，在导入时要加{},而export default则不需要

箭头函数：
1.箭头函数中的this的指向时固定不变的，即是定义函数时的指向
2.普通函数中的this的指向是变化的，即是使用函数时的指向
class继承：
可通过extends关键字实现继承。

```javascript
class A{
    constructor(){
        this.company=‘A’;
    }
    testA(){
        return this.company;
    }
}
class B extends A{
    constructor(){
        super();
        this.exployee='B';
    }
}
```

super关键字，表示父类的构造函数，用来新建父类的this对象。super虽然代表类父类A的构造函数，但是返回的是子类B的实例，即super内部的this指向的是B，因此 super()在这里相当于A.prototype.constructor.call(this)

+ es5和es6实现继承的不同：

es5的继承，实质上是先创建子类的实例对象this,然后将父类的方法添加到this上，Parent.apply(this)
es6的继承，实质上是先创建父类的实例对象this，然后再用子类的构造函数修改this。

+ proxy:

vue3将会用proxy代替object.defineProperty()完成数据劫持工作，会使初始化速度加倍，内存占用减半。
var proxy = new Proxy(target,handler);target表示要拦截的对象，handler用来定制拦截的行为。

### 30.闭包

闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常用方式，就是一个函数中创建另一个函数。

```javascript
// 第一段：
var num = 20;
function fun(){
    var num = 10;
    return function con(){
        console.log( this.num )
    }
}
var funOne = fun();

funOne();  // 20


// 第二段
var num = 20;
function fun(){
    var num = 10;
    return function con(){
        console.log( num )
    }
}
var funOne = fun();

funOne(); // 10
```

### 31.函数柯里化

就是把多个参数的函数，转化为单参数函数。
add(2)(3)(4)(5)输出14:

```javascript
function add(num){
    var sum=0;
    sum= sum+num;
    return function tempFun(numB){
        if(arguments.length===0){
            return sum;
        }else{
            sum= sum+ numB;
            return tempFun;
        }
    }
}
```

