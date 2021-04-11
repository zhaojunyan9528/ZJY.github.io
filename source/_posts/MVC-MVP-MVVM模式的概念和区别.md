---
title: MVC/MVP/MVVM模式的概念和区别
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-04-07 09:01:28
---

1.MVC模式

一种使用MVC（模型-视图-控制器 model-view-controller）设计的web应用程序的模式：

Model：模型，表示应用程序核心（数据库）
View：视图，显示效果（html页面）
controller：控制器，处理用户输入（业务逻辑）

MVC 模式同时提供了对 HTML、CSS 和 JavaScript 的完全控制
最典型的MVC就是JSP + servlet + javabean的模式

视图是用户看到并与之交互的界面
模型表示企业数据和业务规则
控制器接受用户的输入并调用模型和视图去完成用户的需求

MVC是单向通信。也就是View跟Model，必须通过Controller来承上启下

![image](/images/mvc.png)

2.MVP模式

Model-View-Presenter ；MVP 是从经典的模式MVC演变而来，它们的基本思想有相通的地方Controller/Presenter负责逻辑的处理，Model提供数据，View负责显示。

![image](/images/mvp.png)

MVP与MVC有着一个重大的区别：在MVP中View并不直接使用Model，它们之间的通信是通过Presenter (MVC中的Controller)来进行的，所有的交互都发生在Presenter内部，而在MVC中View会直接从Model中读取数据而不是通过 Controller

在MVC模型里，Model不依赖于View，但是View是依赖于Model的

3.MVVM模式

M - Model，Model 代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑

V - View，View 代表 UI 组件，它负责将数据模型转化为 UI 展现出来

VM - ViewModel，ViewModel 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步 View 和 Model 的对象，连接 Model 和 View之间的桥梁，绑定数据到viewmodel层并自动更新渲染到页面上，视图变化通知到viewmodel层去更新数据

![image](/images/mvvm.png)

MVVM模式和MVC模式一样，主要目的是分离视图（View）和模型（Model）

mvvm模式将Presener改名为View Model，基本上与MVP模式完全一致，唯一的区别是，它采用双向绑定(data-binding): View的 变动，自动反映在View Model，反之亦然。这样开发者就不用处理接收事件和View更新的工作

这些模式是依次进化而形成MVC/MVP—>MVVM。在以前传统的开发模式当中即MVC模式，前端人员只负责Model（数据库） View（视图） Controller /Presenter/ViewModel（控制器） 当中的View（视图）部分，写好页面交由后端创建渲染模板并提供数据，随着MVVM模式的出现前端已经可以自己写业务逻辑以及渲染模板，后端只负责提供数据即可。

4.观察者模式：
在观察者模式中，观察者直接订阅目标事件，在目标发出内容改变的事件后，直接接收事件并作出响应

5.发布订阅者模式
在发布订阅模式中，发布者和订阅者之间多了一个发布通道；一方面从发布者接收事件，另一方面向订阅者发布事件；订阅者需要从事件通道订阅事件

观察者模式： 观察者（Observer）直接订阅（Subscribe）主题（Subject），而当主题被激活的时候，会触发（Fire Event）观察者里的事件。

发布订阅模式： 订阅者（Subscriber）把自己想订阅的事件注册（Subscribe）到调度中心（Topic），当发布者（Publisher）发布该事件（Publish topic）到调度中心，也就是该事件触发时，由调度中心统一调度（Fire Event）订阅者注册到调度中心的处理代码。
