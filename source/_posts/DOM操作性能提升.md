---
title: DOM操作性能提升
tags:
  - 前端
categories:
  - - 问题&总结
    - HTML
date: 2021-06-09 17:08:14
---

在浏览器中DOM和Javascript通常是独立实现的，因此通过Javascript操作DOM会产生很大的性能消耗，因此需要尽可能地减少DOM操作

主要有以下几种方式：

### 1.使用innerHTML代替DOM方法

将多次DOM操作转换为字符串拼接，并一次性插入页面

### 2.节点克隆

对于一些相同的节点，使用节点克隆(element.cloneNode)而不是节点创建(element.createElement)来创建

### 3.尽可能少地使用HTML集合

以下方法返回的就是一个集合：

+ document.getElementsByName()
+ document.getElementsByClassName()
+ document.getElementsByTagName()
+ document.images
+ document.links
+ document.forms
+ document.forms[0].elements

HTML集以一种"假定实时态"实时存在，当底层文档对象更新时，它也会自动更新

一种可行的方法是将集的属性存入缓存变量中，或者将HTML集拷贝到普通数组

### 4.减少渲染树的排队和刷新

获取页面布局信息的操作会导致队列刷新，如以下方法：

+ offsetTop，offsetLeft，offsetWidth，offsetHeight
+ scrollTop，scrollLeft，scrollWith，scrollHeight
+ clientTop，clientLeft，clientWidth，clientHeight
+ getComputedStyle()

尽量避免使用以上的方法

即使需要获取布局信息，也要将它保存在局部变量中，如：

```js
// 低效的
element.style.left = 1 + element.offsetLeft + 'px';
element.style.top = 1 + element.offsetTop + 'px';
if (element.offsetLeft >= 500) {
  // dosomething();
}

// 高效的
let currentLeft = element.offsetLeft,
    currentTop = element.offsetTop;
currentLeft ++;
currentTop ++;
element.style.left = current + 'px';
element.style.top = current + 'px';
if (currentLeft >= 500) {
  // dosomething();
}
```

### 5.批量修改dom

提升方式：

+ 使元素脱离文档流
+ 对其应用多重改变
+ 把元素带回文档中
  
使DOM脱离文档的方式：

+ 隐藏元素，应用修改，重新显示
+ 使用文档片段(document fragment)在当前DOM之外构建一个子树，再把它拷贝回文档
+ 将原始元素拷贝到一个脱离文档的节点中，修改副本，完成后再替换元素
  
推荐尽可能地使用第二种方式，因为所产生的DOM遍历和重排次数最少

### 6.事件委托

当页面中有大量元素需要绑定事件处理器，尽可能使用事件委托。它基于这样一个事实：事件逐层冒泡并能被父级元素捕获。使用事件代理，只需给外层元素绑定一个处理器，就可以处理在其子元素上触发的所有事件。

每个事件经历的阶段：

1.捕获阶段：事件从window对象自上而下向目标节点传播的阶段
2.目标阶段：真正的目标节点正在处理事件的阶段
3.冒泡阶段：事件从目标节点自下而上向window对象传播的阶段

事件委托又叫事件代理。基于冒泡，将事件监听器添加到父元素上，而不是每个子元素上。

事件委托的优点：

+ 节省内存占用，减少事件注册
+ 新增子对象时无需再次对其绑定事件

### 7.使用CSS3动画代替JS动画

CSS3动画的性能优于JS动画

### 8.使用hashchange事件代替轮询

使用hashchange事件代替轮询，可以避免页面跳转，提高用户体验

### 9.使用requestAnimationFrame

requestAnimationFrame是浏览器提供的一个API，可以让开发者以一种更有效率的方式来执行动画。它会在浏览器下一次重绘之前，请求浏览器调用指定的函数更新动画。

使用requestAnimationFrame的优点：

+ 节省CPU资源
+ 动画效果更加平滑
+ 兼容性好

使用requestAnimationFrame的缺点：

+ 兼容性不好
+ 无法直接控制动画的暂停和恢复
