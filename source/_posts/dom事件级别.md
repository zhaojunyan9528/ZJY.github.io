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

使用DOM0方式为事件程序赋值时，所赋函数被视为元素的方法。因此，事件处理程序会在元素的作用域运行，即this等于元素。

```html
<input type="button" name="" id="btn1" value="btn1"  />
```

```js
document.getElementById('btn1').onclick = function(){
  console.log(this.id)l //"btn1"
  console.log('click me: dom0');
}
document.getElementById('btn1').onclick = function(){
  console.log('click me: dom0-111');
}
```

点击btn1时，输出'click me: dom0-111'，dom0只支持给一个事件添加一个处理程序.dom0级事件处理程序在元素的作用域运行。通过设置btn.onclick = null;可移除事件处理程序。

## dom2级事件

el.addEventListener(event-name, callback, useCapture);
事件名，事件处理函数，布尔值（false默认值，冒泡阶段调用事件处理函数，true：捕获阶段调用事件处理函数）

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

用addEventListener函数添加的事件必须用removeEventListener()函数移除，且传参数一致。这意味着使用addEventListener添加的匿名函数无法移除，如下：

```js
let btn = document.getElementById('btn1');
btn.addEventListener('click',()=>{
  console.log('click me: addEventListener111')
});
//没有效果
btn.removeEventListener('click',function(){
  console.log('click me: addEventListener111')
})
```

传给removeEventListener()的事件处理函数必须与传给addEventListener()的是同一个，如下：

```js
let btn = document.getElementById('btn1');
let handler = function(){
  console.log(this.id);
}
btn.addEventListener('click',handler,false);
//没有效果
btn.removeEventListener('click',handler,false);//有效果
```

<b>IE事件处理程序</b>
attachEvent()和detachEvent()接收同样的参数：事件处理程序的名字和事件处理函数。因为IE8及更早版本只支持冒泡，所以使用attachEvent添加的事件处理程序只会添加到冒泡阶段。

```js
let btn = document.getElementById('btn1');
btn.attachEvent("onclick",function(){
  console.log("clicked");
});
btn.attachEvent("onclick",function(){
  console.log("hello world");
});
```

attachEvent()的第一个参数是"onclick"而不是“click"

事件触发是反向触发，先打印“hello world“再打印"clicked".

使用detachEvent()来移除事件处理程序，只要提供和attachEvent相同的参数。

作为事件处理程序添加的匿名函数也无法移除。

且attachEvent()添加事件处理程序是在全局作用域运行的。因此this === window

```js
btn.attachEvent("onclick",function(){
  console.log(this === window); //true
});
```


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

## 事件对象

event对象是传递给事件处理程序的唯一参数，不管以那种方式指定事件处理程序。

event.type:执行事件的类型
event.eventPhase:用于确定事件流当前所处的阶段。1:捕获阶段被调用  2:目标阶段被调用  3:冒泡阶段被调用。

在IE中，dom事件对象如果是使用dom0方式指定的，event对象只是window对象的一个属性:

```js
let btn = document.getElementById('btn1');
btn.onclick = function(){
  let event = window.event;
  console.log(event.type);
}
```

如果是attachEvent指定的事件处理程序，event对象仍然是window对新的属性，但是出于方便也将其作为参数传入。
## DOM事件模型和事件流

DOM事件模型分为捕获和冒泡。一个事件发生后，会在子元素和父元素之间传播（propagation）。这种传播分成三个阶段

1.捕获阶段：事件从window对象自上而下向目标节点传播的阶段
2.目标阶段：真正的目标节点正在处理事件的阶段
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

event.preventDefault();阻止默认事件的发生，比如链接跳转，表单提交
event.stopPropagation();阻止事件流在dom结构中传播，取消后续的事件捕获或冒泡。
event.stopImmediatePropagation();用于取消后续事件捕获或事件冒泡，并阻止调用任何后续事件处理程序。

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