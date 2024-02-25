---
title: ECMAScript和JavaScript的区别以及小程序区别
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-19 21:59:22
---


### ECMAScript

ECMAScript是一种由Ecma国际通过ECMA-262标准化的脚本程序设计语言， JavaScript 是 ECMAScript 的一种实现.

小程序中的 JavaScript同浏览器中的 JavaScript 以及 NodeJS 中的 JavaScript 是不相同的

### ECMA-262 规定了 ECMAScript 语言的几个重要组成部分：

+ 语法
+ 类型
+ 语句
+ 关键字
+ 操作符
+ 对象

### 浏览器中JavaScript 构成如下: ECMAScript + DOM（文档对象模型） + BOM（浏览器对象模型）

### NodeJS中JavaScript 构成如下: ECMAScript + NPM (包管理系统) + Native

### 小程序中 JavaScript 构成如下: ECMAScript + 小程序框架 + 小程序 API

同浏览器中的JavaScript 相比没有 BOM 以及 DOM 对象，所以类似 JQuery、Zepto这种浏览器类库是无法在小程序中运行起来的，同样的缺少 Native 模块和NPM包管理的机制，小程序中无法加载原生库，也无法直接使用大部分的 NPM 包。

### 小程序的执行环境

+ 小程序目前可以运行在三大平台：
+ iOS平台，包括iOS9、iOS10、iOS11
+ Android平台
+ 小程序IDE

### 模块化

浏览器中，所有 JavaScript 是在运行在同一个作用域下的，定义的参数或者方法可以被后续加载的脚本访问或者改写。同浏览器不同，小程序中可以将任何一个JavaScript 文件作为一个模块，通过module.exports 或者 exports 对外暴露接口。

### 脚本的执行顺序

浏览器中，脚本严格按照加载的顺序执行;而在小程序中的脚本执行顺序有所不同。小程序的执行的入口文件是 app.js 。并且会根据其中 require 的模块顺序决定文件的运行顺序.当 app.js 执行结束后，小程序会按照开发者在 app.json 中定义的 pages 的顺序，逐一执行

### 作用域

同浏览器中运行的脚本文件有所不同，小程序的脚本的作用域同 NodeJS 更为相似。在文件中声明的变量和函数只在该文件中有效，不同的文件中可以声明相同名字的变量和函数，不会互相影响

### 小程序通信模型

小程序的渲染层和逻辑层分别由2个线程管理：渲染层的界面使用了WebView 进行渲染；逻辑层采用JsCore线程运行JS脚本。一个小程序存在多个界面，所以渲染层存在多个WebView线程，这两个线程的通信会经由微信客户端（下文中也会采用Native来代指微信客户端）做中转，逻辑层发送网络请求也经由Native转发

### 数据驱动

WXML可以先转成JS对象，然后再渲染出真正的Dom树

### 双线程下的界面渲染

小程序的逻辑层和渲染层是分开的两个线程。在渲染层，宿主环境会把WXML转化成对应的JS对象，在逻辑层发生数据变更的时候，我们需要通过宿主环境提供的setData方法把数据从逻辑层传递到渲染层，再经过对比前后差异，把差异应用在原来的Dom树上，渲染出正确的UI界面

### 所有页面的脚本逻辑都跑在同一个JsCore线程，页面使用setTimeout或者setInterval的定时器，然后跳转到其他页面时，这些定时器并没有被清除，需要开发者自己在页面离开的时候进行清理

### Page构造器的参数

+ data:   Object   页面的初始数据
+ onLoad: Function 生命周期函数--监听页面加载，触发时机早于onShow和onReady 在页面没被销毁前只会触发1次
+ onShow: Function 生命周期函数--监听页面显示，触发事件早于onReady 一般从别的页面返回到当前页面时，当前页的+ onShow方法都会被调用
+ onReady:Function 生命周期函数--监听页面初次渲染完成  在页面没被销毁前只会触发1次
+ onHide: Function 生命周期函数--监听页面隐藏
+ onUnload:Function 生命周期函数--监听页面卸载
+ onPullDownRefresh:Function 页面相关事件处理函数--监听用户下拉动作
