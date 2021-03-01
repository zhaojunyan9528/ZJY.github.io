---
title: dom事件级别
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 13:40:34
---
## DOM事件级别

DOM级别一共可以分为四个级别：DOM0级、DOM1级、DOM2级和DOM3级。而DOM事件分为3个级别：DOM 0级事件处理，DOM 2级事件处理和DOM 3级事件处理。由于DOM 1级中没有事件的相关内容，所以没有DOM 1级事件

## dom0级事件

不允许为同个元素绑定多个同类dom0级事件，给元素的事件行为绑定方法，这些方法都是在当前元素事件的冒泡或者目标阶段执行的

```html
<input type="button" name="" id="btn1" value="btn1" onclick="console.log('click me:dom0')" />
```

```js
document.getElementById('btn1').onclick = function(){
  console.log('click me: dom1');
}
```

点击btn1时，输出'click me: dom1'，不允许为同个元素绑定多个同类dom0级事件

## dom2级事件

el.addEventListener(event-name, callback, useCapture)

```js
document.getElementById('btn1').addEventListener('click',function(e){
  console.log('click me: addEventListener111')
});
document.getElementById('btn1').addEventListener('click',function(e){
  console.log('click me: addEventListener222')
});
```

点击btn1，依次输出：
click me: addEventListener111
click me: addEventListener222

## dom3级事件：在dom2级事件基础上添加了更多的事件类型。

UI事件:当用户与页面上元素交互时触发,比如:load,scroll,
焦点事件:当元素获得或失去焦点时触发,比如:blur,focus,
鼠标事件:当用户通过鼠标在页面执行操作时触发如：dblclick、mouseup,
滚轮事件，当使用鼠标滚轮或类似设备时触发，如：mousewheel
文本事件，当在文档中输入文本时触发，如：textInput
键盘事件，当用户通过键盘在页面上执行操作时触发，如：keydown、keypress
合成事件，当为IME（输入法编辑器）输入字符时触发，如：compositionstart
变动事件，当底层DOM结构发生变化时触发，如：DOMsubtreeModified
同时DOM3级事件也允许使用者自定义一些事件

## DOM事件模型和事件流

DOM事件模型分为捕获和冒泡。一个事件发生后，会在子元素和父元素之间传播（propagation）。这种传播分成三个阶段

1.捕获阶段：事件从window对象自上而下向目标节点传播的阶段
2.目标阶段：真正的目标阶段正在处理事件的阶段
3.冒泡阶段：事件从目标节点自下而上向window对象传播的阶段

## 事件代理（事件委托）

由于事件会在冒泡阶段传播到父元素上，由父元素监听函数同时处理多个子节点的事件，称为事件代理。
优点：减少内存的消耗提高性能

```html
<ul id="list">
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
</ul>
```

```js
document.getElementById('list').addEventListener('click', function(e){
  // 兼容性处理
  var event = e || window.event;
  var target = event.target || event.srcElement;
  // 判断是否匹配目标元素
  if (target.nodeName.toLocaleLowerCase() === 'li') {
      console.log('the content is: ', target.innerHTML);
  }
})
```

## Event对象常见的应用

event.preventDefault();阻止默认事件的发生，比如链接跳转，表达提交
event.stopPropagation();阻止事件冒泡到父元素,阻止任何父元素事件处理程序被执行
event.stopImmediatePropagation();既能阻止事件向父元素冒泡,也能阻止元素同事件类型的其他监听器被触发

```js
const btn = document.querySelector('#btn1');
btn.addEventListener('click', event => {
  console.log('btn click 1');
  event.stopImmediatePropagation();
});
btn.addEventListener('click', event => {
  console.log('btn click 2');
});
document.body.addEventListener('click', () => {
  console.log('body click');
});
```

点击btn，输出'btn click 1'

## event & currentTarget

```html
<div id="a">
  <div id="b">
      <div id="c">
          <div id="d"></div>
      </div>
  </div>
</div>
```

```js
document.getElementById('a').addEventListener('click',function(e){
    console.log('target: '+ e.target.id+' &curretTarget: '+e.currentTarget.id)
});
document.getElementById('b').addEventListener('click',function(e){
    console.log('target: '+ e.target.id+' &curretTarget: '+e.currentTarget.id)
});
document.getElementById('c').addEventListener('click',function(e){
    console.log('target: '+ e.target.id+' &curretTarget: '+e.currentTarget.id)
});
document.getElementById('d').addEventListener('click',function(e){
    console.log('target: '+ e.target.id+' &curretTarget: '+e.currentTarget.id)
})
```

点击d元素，依次输出：
target: d &curretTarget: d
target: d &curretTarget: c
target: d &curretTarget: b
target: d &curretTarget: a