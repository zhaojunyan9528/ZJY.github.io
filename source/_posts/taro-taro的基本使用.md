---
title: taro-taro的基本使用
tags:
  - 前端
categories:
  - - 笔记
    - taro
date: 2021-01-19 22:15:02
---

### 项目运行:
    微信小程序：
    npm run dev:weapp
    H5:
    npm run dev:h5


### 项目打包
    微信小程序：
    npm run build:weapp
    H5:
    npm run build:h5

### tips
在 Taro 中尺寸单位建议使用 px、 百分比 %，Taro 默认会对所有单位进行转换。在 Taro 中书写尺寸按照 1:1 的关系来进行书写

如果你希望部分 px 单位不被转换成 rpx 或者 rem ，最简单的做法就是在 px 单位中增加一个大写字母，例如 Px 或者 PX 这样，则会被转换插件忽略

Taro 默认以 750px 作为换算尺寸标准

Taro 提供了 API Taro.pxTransform 来做运行时的尺寸转换:Taro.pxTransform(10) // 小程序：rpx，H5：rem

对于头部包含注释 /*postcss-pxtransform disable*/ 的文件，插件不予处理。

externalClasses 需要使用 短横线命名法 (kebab-case)，而不是 React 惯用的 驼峰命名法 (camelCase)。否则无效。

 Taro 中，所有组件都应当首字母大写并且使用大驼峰式命名法（Camel-Case）

 Taro只能使用map,forEach,reduce等无效

 setState() 函数是唯一能够更新 this.state 的地方。taro的状态更新一定是异步的，可以在回调中取得更新后的值，react的setState不总是异步的

 在 Taro 中不能使用 catchEvent 的方式阻止事件冒泡。你必须明确的使用 stopPropagation

 在 Taro 的页面和组件类中，this 指向的是 Taro 页面或组件的实例,要获取对应的小程序原生页面和组件的实例，通过this.$scope

### react组件的生命周期：

```javascript
    class Clock extends React.Component {

        // constructor，顾名思义，组件的构造函数。一般会在这里进行 state 的初始化，事件的绑定等等

        constructor(props) {

            super(props);

            this.state = {date: new Date()};

        }

        // 是当组件在进行挂载操作前，执行的函数，一般紧跟着 constructor 函数后执行

        componentWillMount() {}

        // 是当组件挂载在 dom 节点后执行。一般会在这里执行一些异步数据的拉取等动作

        componentDidMount() {}

        // 当组件在进行更新之前，会执行的函数

        componentWillUpdate(nextProps, nextState) {}

        // 当组件收到新的 props 时会执行的函数，传入的参数就是 nextProps ，你可以在这里根据新的 props 来执行一些相关的操作，例如某些功能初始化等

        componentWillReceiveProps(nextProps) {}  

        // 当组件完成更新时，会执行的函数，传入两个参数是 prevProps 、prevState

        componentDidUpdate(prevProps, prevState) {}

        // 返回 false 时，组件将不会进行更新，可用于渲染优化

        shouldComponentUpdate(nextProps, nextState) {}

        // 当组件准备销毁时执行。在这里一般可以执行一些回收的工作，例如 clearInterval(this.timer) 这种对定时器的回收操作

        componentWillUnmount() {}
        
        render() {

            return (

            <div>

                <h1>Hello, world!</h1>

                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>

            </div>

            );
        }
    }
```

props：父组件传给子组件的数据，会挂载在子组件的 this.props 上

state：与 props 不同，是属于组件自己内部的数据状态，一般在 constructor 构造函数里初始化定义 state

react组件的三个声明周期状态：

    *Mount: 插入真实Dom
    *Update: 被重新渲染
    *Unmount：被移除真实Dom

### 生命周期流程

    第一次初始化渲染显示：ReactDom.render()

        *constructor: 创建对象初始化state
        *componentWillMount(): 将要插入回调
        * render() : 用于插入虚拟 DOM 回调
        * componentDidMount() : 已经插入回调
    
    每次更新 state: this.setSate() 

        * componentWillUpdate() : 将要更新回调
        * render() : 更新(重新渲染
        * componentDidUpdate() : 已经更新回调

### taro-ui使用方式：

    在页面test.tsx中：
        import { AtButton } from 'taro-ui'
        按需引入：@import "~taro-ui/dist/style/components/button.scss";
    全局引入使用：
        在app.scss中：@import "~taro-ui/dist/style/index.scss";
        在页面：import { AtButton } from 'taro-ui'

