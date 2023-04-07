---
title: vue3
tags:
  - 前端
categories:
  - - 笔记
    - vue
date: 2023-04-06 10:17:41
---

# vue3

安装全局vue-cli

```shell
npm install -g @vue/cli
```

查看vue-cli版本

```shell
vue -V
```

查看局部安装的vue-cli版本

```shell
# 首先进入局部安装目录
npx vue -V
```

创建vue项目

```shell
npx vue create my-project
cd my-project
npm run serve
```

## 全局API

### createApp创建应用实例

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
createApp(App).use(router).mount('#app')
// app.mount()将应用实例挂载在一个容器元素中。
```

## 组合式 API

### setup

setup() 钩子是在组件中使用组合式 API 的入口，通常只在以下情况下使用：

+ 需要在非单文件组件中使用组合式 API 时。
+ 需要在基于选项式 API 的组件中集成基于组合式 API 的代码时。

在模板中访问从 setup 返回的 ref 时，它会自动浅层解包，因此你无须再在模板中为它写 .value

setup() 自身并不含对组件实例的访问权，即在 setup() 中访问 this 会是 undefined。你可以在选项式 API 中访问组合式 API 暴露的值，但反过来则不行。

setup返回的对象会和data和methods合并，setup优先级更高。

setup() 应该同步地返回一个对象。唯一可以使用 async setup() 的情况是，该组件是 Suspense 组件的后裔。

### ref()

接受一个内部值，返回一个响应式的、可更改的 ref 对象，此对象只有一个指向其内部值的属性 .value.

如果将一个对象赋值给 ref，那么这个对象将通过 reactive() 转为具有深层次响应式的对象。这也意味着如果对象中包含了嵌套的 ref，它们将被深层地解包.

```js
// 需要通过value属性访问值
const count = ref(0)
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

```vue
<template>
  <h1>c: {{ a }}</h1>
  <h1>d: {{ d.msg }}</h1>
</template>
<script>
  setup() {
    let a = ref(1)
    let b = ref(1)
    const d = ref({
      msg: 'hello'
    })
    const handleClick = () => {
      a.value++
      b.value++
      d.value.msg = 'test'
      console.log('handleClick-setup')
      // ElMessage.info('handleClick-setip')
    }
    return {
      a,
      b,
      d,
      handleClick
    }
  }
</script>
```

### reactive()

返回一个对象的响应式代理。

响应式转换是“深层”的：它会影响到所有嵌套的属性。一个响应式对象也将深层地解包任何 ref 属性，同时保持响应性。

值得注意的是，当访问到某个响应式数组或 Map 这样的原生集合类型中的 ref 元素时，不会执行 ref 的解包。

```js
// 创建一个响应式对象：
const obj = reactive({ count: 0 })
obj.count++
console.log(obj.count) // 1  无需通过.value属性访问值
```

ref 的解包：

```js
const count = ref(1)
const obj = reactive({ count })

// ref 会被解包
console.log(obj.count === count.value) // true

// 会更新 `obj.count`
count.value++
console.log(count.value) // 2
console.log(obj.count) // 2

// 也会更新 `count` ref
obj.count++
console.log(obj.count) // 3
console.log(count.value) // 3
```

注意当访问到某个响应式数组或 Map 这样的原生集合类型中的 ref 元素时，不会执行 ref 的解包：

```js
const books = reactive([ref('Vue 3 Guide')])
// 这里需要 .value
console.log(books[0].value) // Vue 3 Guide

const map = reactive(new Map([['count', ref(0)]]))
// 这里需要 .value
console.log(map.get('count')?.value) // 0
```

### toRefs()

将一个响应式对象转换为一个普通对象，这个普通对象的每个属性都是指向源对象相应属性的 ref。每个单独的 ref 都是使用 toRef() 创建的.

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const stateAsRefs = toRefs(state)
/*
stateAsRefs 的类型：{
  foo: Ref<number>,
  bar: Ref<number>
}
*/

// 这个 ref 和源属性已经“链接上了”
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) // 3
```

toRefs 在调用时只会为源对象上可以枚举的属性创建 ref。如果要为可能还不存在的属性创建 ref，请改用 toRef.

### toRef

基于响应式对象上的一个属性，创建一个对应的 ref。这样创建的 ref 与其源属性保持同步：改变源属性的值将更新 ref 的值，反之亦然.

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')

// 更改该 ref 会更新源属性
fooRef.value++
console.log(state.foo) // 2

// 更改源属性也会更新该 ref
state.foo++
console.log(fooRef.value) // 3
```

请注意，这不同于：

```js
const fooRef = ref(state.foo)
```

上面这个 ref 不会和 state.foo 保持同步，因为这个 ref() 接收到的是一个纯数值

### 比较vue2和vue3的响应式

vue2的问题：
1.对象直接添加新的属性或删除已有的属性，界面不会自动更新，不是响应式
2.直接通过下标arr[1] = 'xxx'更改元素或数组长度，界面不会自动更新。

以上问题vue3不会出现。

vue2的响应式的核心是通过defineProperty对对象已有的属性值的读取和修改进行劫持

vue3的核心：
通过Proxy代理：拦截对象本身的操作，如属性的添加、删除，值的读写。。
通过Reflect反射：动态对代理对象的属性进行相应的操作

### setup()的参数

setup 函数的第一个参数是组件的 props。和标准的组件一致，一个 setup 函数的 props 是响应式的，并且会在传入新的 props 时同步更新。

```js
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

注意：如果你解构了 props 对象，解构出的变量将会丢失响应性。因此我们推荐通过 props.xxx 的形式来使用其中的 props

如果你确实需要解构 props 对象，或者需要将某个 prop 传到一个外部函数中并保持响应性，那么你可以使用 toRefs() 和 toRef() 这两个工具函数：

```js
setup(props) {
  // 将 `props` 转为一个其中全是 ref 的对象，然后解构
  const { title } = toRefs(props)
  // `title` 是一个追踪着 `props.title` 的 ref
  console.log(title.value)

  // 或者，将 `props` 的单个属性转为一个 ref
  const title = toRef(props, 'title')
}
```

传入 setup 函数的第二个参数是一个 Setup 上下文对象。上下文对象暴露了其他一些在 setup 中可能会用到的值:

```js
setup(props, context) {
  // 透传 Attributes（非响应式的对象，等价于 $attrs）
  console.log(context.attrs)

  // 插槽（非响应式的对象，等价于 $slots）
  console.log(context.slots)

  // 触发事件（函数，等价于 $emit）
  console.log(context.emit)

  // 暴露公共属性（函数）
  console.log(context.expose)
}

setup(props, { attrs, slots, emit, expose }) {
  ...
}

```

attrs 和 slots 都是有状态的对象，它们总是会随着组件自身的更新而更新。这意味着你应当避免解构它们，并始终通过 attrs.x 或 slots.x 的形式使用其中的属性。此外还需注意，和 props 不同，attrs 和 slots 的属性都不是响应式的。如果你想要基于 attrs 或 slots 的改变来执行副作用，那么你应该在 onBeforeUpdate 生命周期钩子中编写相关逻辑。

暴露公共属性:expose 函数用于显式地限制该组件暴露出的属性，当父组件通过模板引用访问该组件的实例时，将仅能访问 expose 函数暴露出的内容：

```js
setup(props, { expose }) {
  // 让组件实例处于 “关闭状态”
  // 即不向父组件暴露任何东西
  expose()

  const publicCount = ref(0)
  const privateCount = ref(0)
  // 有选择地暴露局部状态
  expose({ count: publicCount })
}
```

### computed()

接受一个 getter 函数，返回一个只读的响应式 ref 对象。
该 ref 通过 .value 暴露 getter 函数的返回值。
它也可以接受一个带有 get 和 set 函数的对象来创建一个可写的 ref 对象。

创建一个只读的计算属性 ref：

```js
const count = ref(1)
const plusOne = computed(() => count.value + 1)

console.log(plusOne.value) // 2

plusOne.value++ // 错误
```

创建一个可写的计算属性 ref：

```vue
<template>
  <el-input v-model="firstname" placeholder="firstname"></el-input><br>
  <el-input v-model="secondname" placeholder="secondname"></el-input><br>
  <div>{{ fullname }}</div>
  <el-button @click="handleUpdate">update</el-button>
</template>
<script>
setup() {
  const user = reactive({
    firstname: 'john',
    secondname: 'lucy'
  })
  const fullname = computed({
    get: () => user.firstname + '-' + user.secondname,
    set: (val) => {
    [user.firstname, user.secondname] = val.split('-')
    }
  })
  const handleUpdate = () => {
    fullname.value = 'jay-young'
  }
  return {
    ...toRefs(user),
    fullname,
    handleUpdate
  }
}
</script>

```

### watch()

侦听一个或多个响应式数据源，并在数据源变化时调用所给的回调函数。

watch() 默认是懒侦听的，即仅在侦听源发生变化时才执行回调函数。

第一个参数是侦听器的源.
第二个参数是在发生变化时要调用的回调函数
第三个可选的参数是一个对象，支持以下这些选项：immediate/deep/flush/(onTrack / onTrigger)

侦听一个 getter 函数：

```js
// 监听响应式对象时需要使用函数
const state = reactive({ count: 0 })
watch(
  () => state.count,
  (count, prevCount) => {
    /* ... */
  }
)
// 当直接侦听一个响应式对象时，侦听器会自动启用深层模式：
watch(state, () => {
  /* 深层级变更状态所触发的回调 */
})
```

侦听一个 ref：

```js
const count = ref(0)
watch(count, (count, prevCount) => {
  /* ... */
})
```

当侦听多个来源时，回调函数接受两个数组，分别对应来源数组中的新值和旧值：

```js
watch([fooRef, barRef], ([foo, bar], [prevFoo, prevBar]) => {
  /* ... */
})
```

### watchEffect()

立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行。

不需要侦听器的源，只要其依赖改变就更新，immediate值默认为true

```js
let fullname = ref('')
watchEffect(() => {
  fullname.value = user.firstname + '-' + user.secondname
})
```

### 生命周期钩子

onBeforeMount(): 注册一个钩子，在组件被挂载之前被调用。
onMounted(): 注册一个回调函数，在组件挂载完成后执行。

onBeforeUpdate()​: 注册一个钩子，在组件即将因为响应式状态变更而更新其 DOM 树之前调用.
onUpdated(): 注册一个回调函数，在组件因为响应式状态变更而更新其 DOM 树之后调用。

onBeforeUnmount()​: 注册一个钩子，在组件实例被卸载之前调用。
onUnmounted(): 注册一个回调函数，在组件实例被卸载之后调用。

onActivated()​: 注册一个回调函数，若组件实例是 `<KeepAlive>` 缓存树的一部分，当组件被插入到 DOM 中时调用。
onDeactivated()​: 注册一个回调函数，若组件实例是 `<KeepAlive>` 缓存树的一部分，当组件从 DOM 中被移除时调用

onErrorCaptured()​: 注册一个钩子，在捕获了后代组件传递的错误时调用。

vue2和vue3生命周期对比：

|vue2生命周期 | vue3生命周期 |
|----------|----------|
|beforeCreated|setup|
|created|setup|
|beforeMounted|onBeforeMount|
|mounted|onMounted|
|beforeUpdate|onBeforeUpdate|
|updated|onUpdated|
|beforeDestroy|onBeforeUnmount|
|destroyed|onUnmounted|
|activated|onActivated|
|deactivated|onDeactivated|

### ref获取元素

```vue
<template>
  <el-input v-model="secondname" ref="inputRef"></el-input>
</template>
<script>
setup() {
  const inputRef = ref<HTMLElement | null>(null)
  // 生命周期
  onMounted() {
    inputRef.value && inputRef.value.focus()
  }
  // 或者
  nextTick(() => {
    inputRef.value && inputRef.value.focus()
  })

  return {
    inputRef
  }
}
</script>  
```

### 自定义hook函数

ref, computed, watch, reactive, onMounted等都是hook函数，不过是vue内置hook.

创建一个函数，必须用use开头
函数必须return一些数据

```vue
<template>
{{ x }} - {{ y }}
</template>
<script>
function useMousePostion() {
  const x = ref(-1)
  const y = ref(-1)

  const updatePosition = (e: MouseEvent) => {
    x.value = e.pageX
    y.value = e.pageY
  }

  onMounted(() => {
    document.addEventListener('click', updatePosition)
  })
  onUnmounted(() =>{
    document.removeEventListener('click', updatePosition)
  })

  return {
    x, y
  }
}
setup() {
  const { x, y } = useMousePostion()
  return {
    x,
    y
  }
}
</script>
```

### shallowReactive()和shallowRef()

shallowReactive： reactive() 的浅层作用形式。

和 reactive() 不同，这里没有深层级的转换：一个浅层响应式对象里只有根级别的属性是响应式的。属性的值会被原样存储和暴露，这也意味着值为 ref 的属性不会被自动解包了。

```js
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 更改状态自身的属性是响应式的
state.foo++

// ...但下层嵌套对象不会被转为响应式
isReactive(state.nested) // false

// 不是响应式的
state.nested.bar++
```

和shallowRef: ref() 的浅层作用形式。

和 ref() 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 .value 的访问是响应式的

```js
const state = shallowRef({ count: 1 })

// 不会触发更改
state.value.count = 2

// 会触发更改
state.value = { count: 2 }
```

### toRaw()和markRaw()

toRaw(): 根据一个 Vue 创建的代理返回其原始对象。

```js
const foo = {}
const reactiveFoo = reactive(foo)

console.log(toRaw(reactiveFoo) === foo) // true
```

markRaw(): 将一个对象标记为不可被转为代理。返回该对象本身

```js
const foo = markRaw({})
console.log(isReactive(reactive(foo))) // false

// 也适用于嵌套在其他响应性对象
const bar = reactive({ foo })
console.log(isReactive(bar.foo)) // false
```

### 手动实现reactive

```js
const handler = {
  get(target, prop) {
    console.log('劫持get()', prop)
    return Reflect.get(target, prop)
  },

  set(target, prop, val) {
    console.log('劫持set()', prop, val)
    return Reflect.set(target, prop, val)
  },

  deleteProperty(target, prop) {
    console.log('劫持deleteProperty()', prop)
    return Reflect.deleteProperty(target, prop)
  }
}
function reactive(target) {
  if (target && typeof target === 'object') {
    Object.entries(target).forEach(([key, value]) => {
      if (typeof value === 'object') {
        target[key] = reactive(value)
      }
    })
    return new Proxy(target, handler)
  }
}

function shallowReactive(target) {
  if (target && typeof target === 'object') {
    return new Proxy(target, handler)
  }
}

const state = reactive({
  a: 1,
  b: 2,
  c: {
    d: 3
  }
})
state.a // 劫持get() a
state.c.d // 劫持get() c   劫持get() d
state.c.d = 33 // 劫持get() c   劫持set() d 33

const shallowstate = shallowReactive({
  a: 1,
  b: 2,
  c: {
    d: 3
  }
})
shallowstate.a // 劫持get() a
shallowstate.c.d // 劫持get() c
shallowstate.c.d = 33 // 劫持get() c
```

### 手动实现ref

```js
// 接受一个内部值，返回一个响应式的、可更改的 ref 对象，此对象只有一个指向其内部值的属性 .value。
// 如果将一个对象赋值给 ref，那么这个对象将通过 reactive() 转为具有深层次响应式的对象
function ref(target) {
  if (target && typeof target === 'object') {
    target = reactive(target)
  }
  return {
    _value: target,
    get value() {
      console.log('获取get', this._value)
      return this._value
    },
    set value(val) {
      console.log('设置set', val)
      this._value = val
    }
  }
}
// 和 ref() 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 .value 的访问是响应式的
function shallowRef(target) {
  return {
    _value: target,
    get value() {
      console.log('获取get', this._value)
      return this._value
    },
    set value(val) {
      console.log('设置set', val)
      this._value = val
    }
  }
}
```

## 新组件和API

1.Fragment片段

vue2: 必须有一个根标签
vue3: 组件可以没有根标签，多个组件包含在template中
好处： 减少标签层级，减少内存占用

2.Teleport瞬移

让组件的html可以在父界面外的组件任一标签处插入

3.Suspense异步组件

### 1.新的全局API

+ createApp()
+ app.mount()
+ app.provide()
+ app.component()
+ app.directive()
+ app.use()
+ app.mixin()
+ app.version
+ app.config

### v-model的变化

在表单上使用没有变化。
在组件上使用的时候，默认的属性名和事件变化, sync修饰符移除，使用v-model

```js
// modelValue
<Child v-model="msg" />

// Child.vue
props: ['modelValue'] // value => modelValue
emit("update: modelValue", '111')

// name
<Child v-model:name="msg" />

// Child.vue
props: ['name'] // value => modelValue
emit("update: name", '111')
```

### v-if 与 v-for 一起使用,v-if优先级更高（vue2版本v-for优先级高）
