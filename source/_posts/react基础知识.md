---
title: react基础知识
tags:
  - 前端
categories:
  - - 笔记
    - React
date: 2023-04-11 16:31:41
---

## React

React 是一个用于构建用户界面的 JavaScript 库。

+ 原生js操作Dom繁琐、效率低
+ 原生js直接操作dom，浏览器进行大量重绘重拍
+ 原生js没有组件化编码，代码复用率低

React的特点：

+ 采用组件化模式、声明式编码，提高开发效率和组件复用率
+ 在React Native中使用React语法进行移动端开发
+ 使用虚拟dom+diff算法，减少与真实dom的交互

### 1.使用react(jsx)

```html
<div id="like_button_container"></div>

<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
<!-- 使用jsx -->
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
<!-- 可以在任何 <script> 标签内使用 JSX，方法是在为其添加 type="text/babel" 属性 -->
<script type="text/babel">
  const vDom = (
    <h1>
      <span>hello react</span>
    </h1>
  )
  const domContainer = document.querySelector('#like_button_container');
  ReactDOM.createRoot(domContainer).render(vDom)
</script>
```

虚拟dom是object类型的对象，比真实dom轻，最终会渲染成真实dom

```js
console.log(typeof vDom, vDom instanceof Object) // object true
```

### 2.jsx

JSX 是 JavaScript XML 的简写，表示 JavaScript 代码中写 XML（HTML）格式的代码。一种在React组件内部构建标签的类XML语法。

语法规则：
  定义虚拟dom时，不要写引号，使用小括号()
  使用jsx表达式时，用{}包裹
  样式的类名，使用className属性, style内联样式使用`{{ }}`
  只有一个根标签
  标签必须闭合
  标签首字母：若小写字母开头，则转为html标签同名元素，如果没有对应同名元素则报错；若大写字母开头，就去渲染对应的组件，如果没有定义就报错

```js
const data = ['Angular', 'React', 'Vue']
const listItems = data.map((item) =>
  <li key={item}>{item}</li>
)
const vDom = (
  <div>
    <h1>xxxxxx</h1>
    <ul>
      { listItems }
    </ul>
  </div>
)
const domContainer = document.querySelector('#test');
ReactDOM.createRoot(domContainer).render(vDom)
```

### state和事件处理

```js
  class Person {
    constructor(name, age) {
      // this是类的实例对象
      this.name = name
      this.age = age
    }
    speak() {
      // 方法放在了类的原型对象上，Person.prototype.speak
      // 通过实例对象调用的方法，this就是实例对象
      console.log(`my name is ${this.name}, age is ${this.age}`)
    }
  }
  // 继承
  class Student extends Person {
    constructor(name, age, sex) {
      // this是类的实例对象
      super(name, age)
      this.sex = sex
    }
  }
  const p1 = new Person('lucy', 20)
  p1.speak()
  console.log(p1) // Person: { name: 'lucy', age: 20, Prototype: {constructor, speak}}

  const s1 = new Student('xiaomi', 22, '男')
  console.log(s1)
  s1.speak()

  class Weather extends React.Component {
    // 如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。
    constructor(props) {
      super(props)
      // 在 constructor() 函数中不要调用 setState() 方法。如果你的组件需要使用内部 state，请直接在构造函数中为 this.state 赋值初始 state
      // 只能在构造函数中直接为 this.state 赋值。如需在其他方法中赋值，你应使用 this.setState() 替代
      this.state = {
        isHot: false
      }
      this.handleClick = this.handleClick.bind(this)
    }
    handleClick(e) {
      // console.log(this)
      // handleClick作为onclick的回调，不是通过实例对象调用的是直接调用的， 所以this是undefined， 需要bind绑定this（实例对象）
      this.setState(prevState => ({
        isHot: !prevState.isHot
      }))
    }
    render() {
      console.log(this)
      return <h1 onClick={this.handleClick}>today is {this.state.isHot ? 'hot': 'cold'}</h1>
    }
  }
  ReactDOM.createRoot(document.getElementById('test')).render(<Weather />)
```

state不可直接更改，通过setState更改，合并更新;
constructor构造器只执行一次，render调用1-n次;

通常，在 React 中，构造函数仅用于以下两种情况：

+ 通过给 this.state 赋值对象来初始化内部 state。
+ 为事件处理函数绑定实例

简写state和方法调用：

```js
state = { isHot: false } // 不需要contructor

render() {
  console.log(this)
  return <h1 onClick={(e) => this.handleClick()}>today is {this.state.isHot ? 'hot': 'cold'}</h1>
}
/* 或者 */
handleClick = (e) => {
  this.setState(prevState => ({
    isHot: !prevState.isHot
  }))
}
```

### props

```js
class Woman extends React.Component {
  render() {
    const { name, age } = this.props
    return (
      <ul>
        <li>name: { name }</li>
        <li>age: { age }</li>
      </ul>
    )
  }
}
const p = { name: 'lucy', age: 20 }
ReactDOM.createRoot(document.getElementById('test')).render(<Woman {...p}/>) // 多个props
// ReactDOM.createRoot(document.getElementById('test')).render(<Woman name="lucy"/>) // 单个props
```

约束props类型和必传,默认值：
引入`<script src="https://unpkg.com/prop-types@15.6/prop-types.js"></script>`

```js
Woman.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
}
Woman.defaultProps = {
  name: 'cassie',
  age: 99
}
// 简写：
class Woman extends React.Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number
  }
  static defaultProps = {
    name: 'cassie',
    age: 99
  }
}
```

函数式组件使用props：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
ReactDOM.createRoot(document.getElementById('test')).render(<Welcome name="lucy"/>)
```

**注意：组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props.**

### refs

string类型的refs

```js
// 字符串ref
class Demo extends React.Component {
  handleLeft(e) {
    // const input = document.getElementById('left')
    // alert('input-left: '+ input.value)
    alert('input-left: '+ this.refs.left.value)
  }
  handleBlur(e) {
    // const input = document.getElementById('right')
    // alert('input-right: '+ input.value)
    alert('input-right: '+ this.refs.right.value)
  }
  render() {
    return (
      <div>
        <input ref="left" id="left" type="text" placeholder="please input" />
        <button onClick={(e) => this.handleLeft(e)}>click me left</button>
        <input ref="right" id="right" onBlur={(e) => this.handleBlur(e)} type="text" placeholder="please input" />
      </div>
    )
  }
}
ReactDOM.createRoot(document.getElementById('test')).render(<Demo />)
```

createRef创建的ref和回调ref：

```js
class Demo extends React.Component {
  constructor(props) {
    super(props)
    this.leftRef = React.createRef()
  }
  handleLeft(e) {
    // const input = document.getElementById('left')
    // alert('input-left: '+ input.value) // 原生js
    // alert('input-left: '+ this.refs.left.value) // string类型的refs（过时）
    console.log(this.leftRef);
    alert('input-left: '+ this.leftRef.current.value) // createRef方式
  }
  handleBlur(e) {
    // const input = document.getElementById('right')
    // alert('input-right: '+ input.value)
    // alert('input-right: '+ this.refs.right.value)
    alert('input-right: ' + this.rightRef.value) // 回调refs
  }
  render() {
    return (
      <div>
        <input ref={this.leftRef} id="left" type="text" placeholder="please input" />
        <button onClick={(e) => this.handleLeft(e)}>click me left</button>
        <input ref={current => this.rightRef = current} id="right" onBlur={(e) => this.handleBlur(e)} type="text" placeholder="please input" />
      </div>
    )
  }
}
ReactDOM.createRoot(document.getElementById('test')).render(<Demo />)
```

**关于回调 refs 的说明:**

如果 ref 回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数 null，然后第二次会传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。

通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题，但是大多数情况下它是无关紧要的。

```js
 this.inputText = null
this.rightRef1 = (current) => {
  console.log('回调refs-class-绑定函数', current);
  this.inputText = current
}
<input ref={this.rightRef1} id="right" onBlur={(e) => this.handleBlur1(e)} type="text" placeholder="please input" />
```

### 事件处理

React事件的命名采用小驼峰式camelCase, 而不是纯小写。
使用jsx语法时，需要传入一个函数作为事件处理函数，而不是一个字符串。
不能通过return false的方式阻止默认行为，必须显式使用e.preventDefault().
在js中，class的方法默认不会绑定this，可以使用onClick={() => this.handleClick()}或者handleClick = () => {}来绑定this
通过事件委托方式
向事件处理程序传递参数：

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button> // 箭头函数
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button> // Function.prototype.bind ,事件对象以及更多的参数将会被隐式的传递
```

受控组件：表单数据是由React组件来管理的（state）
非受控组件：表单数据由DOM节点来处理（ref）。

### 生命周期

***挂载***

当组件实例被创建并插入dom时，其生命周期调用顺序如下：

+ contructor()
+ static getDerivedStateFromProps()
+ render()
+ componentDidMount()

当组件的props或state发生变化时会触发更新。更新的生命周期调用顺序如下：

+ static getDerivedStateFromProps()
+ shouldComponentUpdate() 必须有返回值，返回值为假值，则不进行组件更新（强制更新除外）
+ render()
+ getSnapshotBeforeUpdate()
+ componentDidUpdate()
  
***卸载***

当组件从 DOM 中移除时会调用如下方法：

+ componentWillUnmount()

[生命周期图](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

新的生命周期比旧生命周期相比：废弃componentWillMount,componentWillUpdate,componentWillReceiveProps,新增getDerivedStateFromProps、getSnapshotBeforeUpdate

**getDerivedStateFromProps：**

会在调用 render 方法之前调用并且在初始挂载及后续更新时都会被调用。
它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。

**getSnapshotBeforeUpdate：**

getSnapshotBeforeUpdate() 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。
此生命周期方法的任何返回值将作为参数传递给 componentDidUpdate()。

### 配置代理

单个代理：

在package.json里配置proxy：

```json
{
  "proxy": "http://localhost:5000"
}
```

在setupProxy配置多个代理：

```js
const { createProxyMiddleware } = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    createProxyMiddleware('/api1', {
      target: 'http://localhost:5000',
      changeOrigin: true,
      pathRewrite: {
        '^/api1': ''
      }
    }),
    createProxyMiddleware('/api2', {
      target: 'http://localhost:5001',
      changeOrigin: true,
      pathRewrite: {
        '^/api2': ''
      }
    })
  )
}
```

使用需要加前缀：

```js
getData = () => {
  axios.get('http://localhost:3000/api1/students').then(res => {
    console.log('success', res)
  }, error => {
    console.log('error', error)
  })
}
getCarData = () => {
  axios.get('http://localhost:3000/api2/cars').then(res => {
    console.log('success', res)
  }, error => {
    console.log('error', error)
  })
}
```

### 解决多路径刷新页面样式丢失问题

1.public/index.html引入样式不写 ./ 写 /
2.public/index.html引入样式不写 ./ 写 %PUBLIC_URL%
3.使用HashRouter

### Rediect的使用

一般卸载路由注册的最下方，当所有路由都无法匹配时，跳转到redirect所指定的路由

```js
<Switch>
  <Route path="/home" component={Home} />
  <Route path="/about" component={About} />
  <Redirect to="/home" />
</Switch>
```

### 路由组件和一般组件

**路由匹配:**

```js
<Route path="/about" component={About} /> // 路由组件
<About  /> // 一般组件
```

**存放位置:**

一般组件：compoents
路由组件：pages

**接收到props不同：**

一般组件：组件标签传递什么就接收什么
路由组件：接收到3个固定属性history,location,match

```js
history
  action: "PUSH"
  block: ƒ block(prompt)
  createHref: ƒ createHref(location)
  go: ƒ go(n)
  goBack: ƒ goBack()
  goForward: ƒ goForward()
  length: 32
  listen: ƒ listen(listener)
  location: {pathname: '/home', search: '', hash: '', state: undefined, key: 'x2v2h1'}
  push: ƒ push(path, state)
  replace: ƒ replace(path, state)

location:
  hash: ""
  key: "x2v2h1"
  pathname: "/home"
  search: ""
  state: undefined

match:
  isExact: true
  params: {}
  path: "/home"
  url: "/home"
```

### 嵌套路由

1.注册子路由要写父路由的path

```json
--/home
  --/home/news
```

2.路由的匹配是按照注册路由的顺序进行的

### 路由组件传递params参数(地址栏可见)

1.传递

```js
<Link to={`/home/message/detail/${item.id}/${item.title}`}>{item.message}</Link>
```

2.接收

```js
<Route path="/home/message/detail/:id/:title" component={MessageDetail}></Route>
```

3.获取值

```js
const { id, title } = this.props.match.params
```

### 路由组件传递search参数(地址栏可见)

1.传递

```js
<Link to={`/home/message/detail/?id=${item.id}&title=${item.title}`}>{item.message}</Link>
<Link to={{
  pathname: `/home/message/detail`,
  search: `?id=${item.id}&title=${item.title}`
}}>{item.message}</Link>
```

2.接收

```js
<Route path="/home/message/detail" component={MessageDetail}></Route>
```

3.获取值

```js
import qs from 'qs'
const search = qs.parse(this.props.location.search.replace('?', ''))  // result: {id: xxx, title: xxx}
const { id, title } = search
```

### 路由组件传递state参数(地址栏不可见)

1.传递

```js
<Link to={{
  pathname: `/home/message/detail`,
  state: { id: item.id, title: item.title}
}}>{item.message}</Link>
```

2.接收

```js
<Route path="/home/message/detail" component={MessageDetail}></Route>
```

3.获取值

```js
const { id, title } = this.props.location.state  // state: {id: xxx, title: xxx}
```

**参数地址栏不可见，刷新也能保留参数，但是清除浏览器历史记录后state为undefined.**

### 编程式路由导航

```js
// push-search参数
this.props.history.push(`/home/message/detail/?id=${item.id}&title=${item.title}`)
// push-params
this.props.history.push(`/home/message/detail/${item.id}/${item.title}`)
// push-state
this.props.history.push(`/home/message/detail`, {id: 1, title: 1})

// replace-search参数
this.props.history.replace(`/home/message/detail/?id=${item.id}&title=${item.title}`)
// replace-params
this.props.history.replace(`/home/message/detail/${item.id}/${item.title}`)
// replace-state
this.props.history.replace(`/home/message/detail`, {id: 1, title: 1})

// 前进
this.props.history.goForward()
// 后退
this.props.history.goBack()
// 前进/后退
this.props.history.go(n) // n为正负整数，正前进，负后退

```

### withRouter

一般组件没有history,location, match这三个属性，要使用路由可以用withRouter
可以通过 withRouter 高阶组件访问 history 对象的属性和最近的 `<Route>` 的 match 。 当路由渲染时， withRouter 会将已经更新的 match ， location 和 history 属性传递给被包裹的组件.

```js
import React, { Component } from 'react'
import { withRouter } from "react-router-dom"
class HeaderRouter extends Component {
  forward = () => {
    console.log('forward')
    this.props.history.goForward()
  }
  back = () => {
    console.log('back', this.props.history)
    this.props.history.goBack()
  }
  render() {
    return (
      <div>
        <h1>HeaderRouter</h1>
        <button onClick={this.forward}>前进</button>
        <button onClick={this.back}>后退</button>
      </div>
    )
  }
}
export default withRouter(HeaderRouter)
```

### BrowserRouter 和 HashRouter 的区别

**底层原理不一样：**

BrowserRouter使用h5的history API，不兼容ie9及以下版本
HashRouter 使用的是URL的哈希值

**url表现形式不一样：**

BrowserRouter的路径使用/， 例如 "<http://localhost:3000/home/news>"
HashRouter的路径使用#， 例如 "<http://localhost:3000#home/news>"

**刷新后对路由state参数的影响：**

BrowserRouter：没有影响，state保存在history中
HashRouter：导致路由state参数的丢失

### antd 使用

1.安装

```shell
npm install antd --save
yarn add antd
```

2.引入
antd.js 和 antd.min.js 依赖 react、react-dom、dayjs，请确保提前引入这些文件。

```js
// 由于 antd 组件的默认文案是英文，所以需要修改为中文
import dayjs from 'dayjs'
import 'dayjs/locale/zh-cn'
import zhCN from 'antd/locale/zh_CN'
import 'antd/dist/reset.css'
import { ConfigProvider } from 'antd'

dayjs.locale('zh-cn')

render(
  <ConfigProvider locale={zhCN}>
    <App />
  </ConfigProvider>
)
```

### 使用antd-icon图标

1.安装

```shell
npm install --save @ant-design/icons
```

2.引入&使用

```js
import {
  HomeOutlined,
  LoadingOutlined,
  SettingFilled,
  SmileOutlined,
  SyncOutlined,
  SearchOutlined
} from '@ant-design/icons'

<HomeOutlined  style={{ fontSize: '20px', color: 'red' }} />
<LoadingOutlined style={{ fontSize: '20px', color: 'blue' }} />
<SettingFilled  style={{ fontSize: '20px', color: 'green' }} />
<SmileOutlined  style={{ fontSize: '20px', color: 'pink' }} />
<SyncOutlined  style={{ fontSize: '20px', color: 'yellow' }} />

<Button type="primary" icon={<SearchOutlined />}>primary Button</Button>
```

### redux

Redux 是一个使用叫做 “action” 的事件来管理和更新应用状态的模式和工具库 它以集中式 Store（centralized store）的方式对整个应用中使用的状态进行集中管理，其规则确保状态只能以可预测的方式更新。
Redux 使用 "单向数据流

1. 1个专门用于做状态管理的js库，不是react的插件库
2. 它可以用在react、angular、vue等项目中，但基本与react配合使用
3. 作用：集中式管理react应用中共享的状态

**1.store:**

  当前 Redux 应用的 state 存在于一个名为 store 的对象中。
  store 是通过传入一个 reducer 来创建的，并且有一个名为 getState 的方法，它返回当前状态值：

**2.action:**

  action 是一个具有 type 字段的普通 JavaScript 对象。你可以将 action 视为描述应用程序中发生了什么的事件.
  action 对象可以有其他字段，其中包含有关发生的事情的附加信息。按照惯例，我们将该信息放在名为 payload 的字段中。
  action creator 是一个创建并返回一个 action 对象的函数

**3.reducer:**

  reducer 是一个函数，接收当前的 state 和一个 action 对象，必要时决定如何更新状态，并返回新状态。函数签名是：(state, action) => newState。 你可以将 reducer 视为一个事件监听器，它根据接收到的 action（事件）类型处理事件。

Dispatch:Redux store 有一个方法叫 dispatch。更新 state 的唯一方法是调用 store.dispatch() 并传入一个 action 对象。 store 将执行所有 reducer 函数并计算出更新后的 state，调用 getState() 可以获取新 state.
Selector: Selector 函数可以从 store 状态树中提取指定的片段。随着应用变得越来越大，会遇到应用程序的不同部分需要读取相同的数据，selector 可以避免重复这样的读取逻辑

**Redux 数据流:**
早些时候，我们谈到了“单向数据流”，它描述了更新应用程序的以下步骤序列：

+ State 描述了应用程序在特定时间点的状况
+ 基于 state 来渲染视图
+ 当发生某些事情时（例如用户单击按钮），state 会根据发生的事情进行更新
+ 基于新的 state 重新渲染视图
+ 具体来说，对于 Redux，我们可以将这些步骤分解为更详细的内容：

初始启动：

+ 使用最顶层的 root reducer 函数创建 Redux store
+ store 调用一次 root reducer，并将返回值保存为它的初始 state
+ 当视图 首次渲染时，视图组件访问 Redux store 的当前 state，并使用该数据来决定要呈现的内容。同时监听 store 的更新，以便他们可以知道 state 是否已更改。

更新环节：

+ 应用程序中发生了某些事情，例如用户单击按钮
+ dispatch 一个 action 到 Redux store，例如 dispatch({type: 'counter/increment'})
+ store 用之前的 state 和当前的 action 再次运行 reducer 函数，并将返回值保存为新的 state
+ store 通知所有订阅过的视图，通知它们 store 发生更新
+ 每个订阅过 store 数据的视图 组件都会检查它们需要的 state 部分是否被更新。
+ 发现数据被更新的每个组件都强制使用新数据重新渲染，紧接着更新网页

Redux 使用 "单向数据流":

+ State 描述了应用程序在某个时间点的状态，视图基于该 state 渲染
+ 当应用程序中发生某些事情时：
  + 视图 dispatch 一个 action
  + store 调用 reducer，随后根据发生的事情来更新 state
  + store 将 state 发生了变化的情况通知 UI
+ 视图基于新 state 重新渲染

Redux 有这几种类型的代码

+ Action 是有 type 字段的纯对象，描述发生了什么
+ Reducer 是纯函数，基于先前的 state 和 action 来计算新的 state
+ 每当 dispatch 一个 action 后，store 就会调用 root reducer.

(Redux原理图)<https://img1.baidu.com/it/u=549036950,105530499&fm=253&fmt=auto&app=138&f=JPG?w=913&h=500>
(Redux原理图)<https://cn.redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif>

同步action: 一般对象类型{type: '', data: null}为同步action
异步action：返回值为函数为异步action

redux-thunk 来开发特定的异步逻辑（异步action）

redux: js库
react-redux:react插件库

react-redux:
容器组件：负责和redux同学，将结果交给ui组件
ui组件：不能使用任何redux的api，只负责页面交互、呈现

如何创建容器组件，react-redux的connect函数：
容器组件会传给ui组件2个参数：mapStateToProps状态映射为props；映射操作状态的方法

```js
connect(mapStateToProps, mapDispatchToProps)(CountUI)
```

容器中的store是靠props传进去，不是在容器直接引入

```js
function mapStateToProps(state) {
  return {
    count: state
  }
}
function mapDispatchToProps(dispatch) {
  return {
    increment: (number) => { dispatch(createIncrementAction(number)) },
    decrement: (number) => { dispatch(createDecrementAction(number)) },
    incrementAsyc: (number, time) => { dispatch(createIncrementActionAsync(number, time)) }
  }
}
```

connect参数可简写：

```js
export default connect(
  state => ({count: state}), 
  {
    increment: createIncrementAction,
    decrement: createDecrementAction,
    incrementAsyc: createIncrementActionAsync
  })(CountUI)


// index.js
import { Provider } from 'react-redux'
import store from './store'

<Provider store={store}>
  <App />
</Provider>
```

### Redux的reducer一定要是纯函数

纯函数：只要函数的调用参数相同，就永远返回相同的结果。

实现纯函数：

1. 不修改入参
2. 不执行有副作用的操作（修改函数之外的其他变量、API调用等）
3. 不调用其他非纯函数（Date.now()）
  保证纯函数的原则，则每当我们dispatch一个相同的action，且初状态相同，则
  总是能得到一个相同的结果（状态变化），因此可以实现redux的时间旅行功能。

### 使用redux-devtools工具（浏览器插件）

1.安装redux-devtools插件

2.项目安装插件包

```shell
npm install --save redux-devtools-extension
```

3.项目使用

```store.js
import { createStore, applyMiddleware, combineReducers } from 'redux'
// 使用 Thunk Middleware 来做异步 Action
import thunk from 'redux-thunk'
import { composeWithDevTools } from 'redux-devtools-extension' // 此处引入

import countReducer from './reducers/count.js'
import personReducer from './reducers/person.js'

export default createStore(combineReducers({
  count: countReducer,
  person: personReducer
}), composeWithDevTools(applyMiddleware(thunk)))
```

### 打包本地运行

```shell
npm run build
# 生成build文件夹

# static server:
npm install -g serve
serve -s build
```

### setState

**setState更新状态的2种方法：**

setState(nextState, callback?):
  nextState: 对象或函数。如果将一个对象作为nextState传递，它将被浅合并到this.state中。
  callback: 可选的回调函数。如果指定，React将在提交更新后（状态更新界面也更新后）调用您提供的回调。

setState(updater, callback?):
  updater: 更新器函数。它必须是纯的，应该以state和props作为参数，并且应该返回要浅合并到this.state中的对象。React将把你的updater函数放在队列中，并重新呈现你的组件
  callback: 可选的回调函数。如果指定，React将在提交更新后（状态更新界面也更新后）调用您提供的回调。

```js
// 对象式
const { count } = this.state
this.setState({
  count: count + 1
}, () => {
  console.log(this.state.count)
})

// 函数式
// this.setState((state) => ({count: state.count + 1}))
this.setState((state, props) => {
  console.log(state, props)
  return {
    count: state.count + 1
  }
})
```

总结： 对象式的setState是函数式的setState的简写方式，语法糖
使用规则：

1. 如果新状态不依赖于原状态，使用对象式
2. 如果新状态依赖于原状态，使用函数式
3. 如果要在setState执行后获取最新的状态数据，要在callback回调函数中读取

### 路由懒加载(lazy & Suspense)

```js
import React, { Component, lazy, Suspense } from 'react'
// 懒加载
const Home = lazy(() => import('../pages/Home'))
const About = lazy(() => import('../pages/About'))

// 结合：
<Suspense fallback={<h1>Loading...</h1>}>
  <Switch>
    <Route path="/home" component={Home} />
    <Route path="/about" component={About} />
    {/* <Redirect to="/home" /> */}
  </Switch>
</Suspense>
```

### Hooks

+ react16.8版本增加的新特性新语法
+ 可以让你在函数式组件中使用state以及其他react特性

常用hooks：

1. State hook: useState()
2. Effect hook: useEffect()
3. Ref hook: useRef()

**useState():**

```js
const [state, setState] = useState(initalValue)

// 比如：
const [age, setAge] = useState(28);
const [name, setName] = useState('Taylor');
const [todos, setTodos] = useState(() => createTodos());


let [count, setCount] = useState(0)
function add () {
  // setCount(count + 1)
  setCount(count => count + 1)
}
```

**useEffect():**

useEffect(effect: React.EffectCallback, deps?: React.DependencyList | undefined): void

effect hook可以让你在函数组件中执行副作用操作（用于模拟类组件中的生命周期钩子）
effect中的副作用操作：

1. 发ajax请求
2. 设置订阅/定时器
3. 手动更改dom

语法：

```js
useEffect(() => {
  // 在此处可执行副作用操作
  // dosomething...
  return () => { // 在组件卸载前执行
    // 做收尾，比如清除定时器取消订阅
  }
}, [depsList]) // 依赖的state，如果[]则只会在第一次render后执行
```

可以把useEffect hook看成以下三个函数的组合：

+ componentDitMount()
+ componentDidUpdate()
+ componentWillUnmount()

**useRef():**

useRef 是一个 React Hook，它能让你引用一个不需要渲染的值。改变 ref 不会触发重新渲染，所以 ref 不适合用于存储期望显示在屏幕上的信息。如有需要，使用 state 代替

***用 ref 引用一个值:***

```js
import { useRef } from 'react';

export default function Counter() {
  let ref = useRef(0);

  function handleClick() {
    ref.current = ref.current + 1;
    alert('You clicked ' + ref.current + ' times!');
  }

  return (
    <button onClick={handleClick}>
      Click me!
    </button>
  );
}
```

不要在渲染期间写入 或者读取 ref.current

```js
function MyComponent() {
  // ...
  // 🚩 不要在渲染期间写入 ref
  myRef.current = 123;
  // ...
  // 🚩 不要在渲染期间读取 ref
  return <h1>{myOtherRef.current}</h1>;
}
```

事件处理程序或者 effects 中读取和写入 ref。

```js
function MyComponent() {
  // ...
  useEffect(() => {
    // ✅ 你可以在 effects 中读取和写入 ref
    myRef.current = 123;
  });
  // ...
  function handleClick() {
    // ✅ 你可以在事件处理程序中读取和写入 ref
    doSomething(myOtherRef.current);
  }
  // ...
}
```

***通过 ref 操作 DOM:***

```js
// 击按钮将会聚焦 input：
import { useRef } from 'react';

function MyComponent() {
  const inputRef = useRef(null)
  function handleClick() {
    inputRef.current.focus();
  }
  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  )
}
```

获取自定义组件的 ref:

```js
import { forwardRef, useRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

### 文档碎片Fragment

```js
<>
  <OneChild />
  <AnotherChild />
</>
// or
<Fragment>
  <OneChild />
  <AnotherChild />
</Fragment>
// or 遍历添加key
<Fragment key={1}>
  <OneChild />
  <AnotherChild />
</Fragment>
```

### createContext

createContext 能让你创建一个 context 以便组件能够提供和读取。方便后代组件祖组件之间通信。

1.创建上下文

```js
import React, { Component, createContext } from 'react'

// 1.创建上下文
const UsernameContext = createContext()

// 2.传递
<UsernameContext.Provider value={{...this.state}}>
  <B  />
</UsernameContext.Provider>

// 3.接收使用
// c组件
// static contextType适用于类式组件
class C extends Component {
  static contextType = UsernameContext // 声明接收context
  render() {
    return (
      <div style={{margin: '20px 0 0 40px'}}>
        <h1>组件C：</h1>
        <h2>从B组件接收到的username: {this.context.username}  age:{this.context.age}</h2>
      </div>
    )
  }
}
// Consumer适用于函数和类式组件
function C() {
  return (
    <div style={{margin: '20px 0 0 40px'}}>
      <h1>组件C：</h1>
      <h2>从B组件接收到的username: 
        <UsernameContext.Consumer>
          {
            value => `${value.username}   age: ${value.age}`
          }
        </UsernameContext.Consumer>
      </h2>
    </div>
  )
}
```

### PureComponent

PureComponent会在shouldComponentUpdate对props和state进行浅比较，跳过不必要的更新，提高组件性能。
但是props 和 state 不能使用同一个引用。

以下不回触发render：

```js
const obj = this.state
obj.name = 'react'
this.setState(obj)
```

### render props向组件内部传入带内容的标签

vue中使用slot插槽
react中：render props 或者children props

render props:

```js
// app.js
render() {
  return (
    <>
      <h1>app:</h1>
      <A render={(name) => <B name={name}/>} />
    </>
  )
}

class A extends Component {
  state = {
    name: 'react',
  }
  render() {
    return (
      <>
        <h1>A:</h1>
        {this.props.render(this.state.name)}
      </>
    )
  }
}
class B extends Component {
  render() {
    return <h1>我是B: {this.props.name}</h1>
  }
}
```

children props: 不能传递props

```js
// app.js
render() {
  return (
    <>
      <h1>app:</h1>
      <A>
        <B  />
      </A>
    </>
  )
}
class A extends Component {
  state = {
    name: 'react',
  }
  render() {
    return (
      <>
        <h1>A:</h1>
        {this.props.children}
      </>
    )
  }
}
// B...
```

### 错误边界

error boundary错误边界：用来捕获后代组件错误，渲染出备用页面

只能捕获后**代组件生命周期**产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件 定时器中产生的错误

使用：

```js
state = {
  error: ''
}
static getDerivedStateFromError(error) {
  console.log(error)
  return {
    hasError: error
  }
}

componentDidCatch(error, info) {
  // 可以统计页面的错误，发送请求到后台去
  console.log(error, info)
}
```

### 组件通信方式

组件间关系：

+ 父子组件 (props)
+ 兄弟组件（非嵌套组件）(消息订阅-发布 集中式管理)
+ 祖孙组件（跨级组件）(消息订阅-发布 集中式管理 context)

通信方式：

+ props: children props / render props
+ 消息订阅-发布：pubs-sub
+ 集中式管理：redux等
+ context： createContext、Provider、Consumer
