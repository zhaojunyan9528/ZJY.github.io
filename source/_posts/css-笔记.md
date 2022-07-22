---
title: css-笔记
tags:
  - 前端
categories:
  - - 笔记
    - CSS
date: 2022-07-14 15:40:37
---

### 第一章 层叠、优先级和继承

css里的c代表cascade,层叠，层叠决定了如何解决冲突，是css语言的基础。层叠会依据三种条件解决冲突。

(1) 样式表的来源：样式是从哪里来的，包括你的样式和浏览器默认样式等。

(2) 选择器优先级：哪些选择器比另一些选择器更重要。

(3) 源码顺序：样式在样式表里的声明顺序。

声明：color: black;
包含在大括号内的一组声明被称作一个声明块。
声明块前面有一个选择器.
选择器和声明块一起组成了规则集（ruleset）

```css
  body {
    color: black;
  }
```

1. 样式表的来源

  你添加到网页里的样式表并不是浏览器唯一使用的样式表，还有其他类型或来源的样式表。你的样式表属于作者样式表，除此之外还有用户代理样式表，即浏览器默认样式。用户代理样式表优先级低，你的样式会覆盖它们.

  ! important声明会被当作更高优先级的来源，因此总体的优先级按照由高到低排列如下所示：
  (1) 作者的！important
  (2) 作者的样式表
  (3) 用户代理样式表

  层叠规则顺序：不同来源的声明-内联声明-选择器优先级声明-源码顺序声明

2. 理解优先级

  浏览器将优先级分为两部分：HTML的行内样式和选择器的样式。
  
  1.行内样式
  如果用HTML的style属性写样式，这个声明只会作用于当前元素。实际上行内元素属于“带作用域的”声明，它会覆盖任何来自样式表或者`<style>`标签的样式。行内样式没有选择器，因为它们直接作用于所在的元素

  2.选择器优先级
  伪类选择器（:hover）和属性选择器（[type="input"]）与一个类选择器的优先级相同。
  通用选择器（*）和组合选择器（>,+,~）对优先级无影响。

  3.源码顺序
  层叠的第三步，也是最后一步，是源码顺序。如果两个声明的来源和优先级相同，其中一个声明在样式表中出现较晚，或者位于页面较晚引入的样式表中，则该声明胜出。

  4.层叠值
  浏览器遵循三个步骤，即来源，优先级，源码顺序来解析网页上每个元素的每个属性。
  处理层叠的两条通用法则：
    1.在选择器中不要使用ID
    2.不要使用!important

3. 继承

  如果一个元素的某个属性没有层叠值，则可能继承祖先元素的值。比如给`<body>`元素加上font-family,
  就不必给每个元素明确指定字体。但不是所有属性都能被继承。只有特定属性能被继承，主要和文本相关的
  属性：
    color、font、font-family、font-size、font-weight、font-variant、font-style、
    line-height、letter-spacing、text-align、text-indent、text-transform、white-space
    word-spacing

  还有一些其他属性能被继承，比如列表属性：
    list-style、list-style-type、list-style-position、list-style-image

  表格的边框属性: border-collapse、border-spacing 也能被继承

4. 特殊值

  有两个特殊值可以赋给任意属性，用于控制层叠：inherit和initial

  1.inherit关键字
  可以用继承代替一个层叠值，比如覆盖另一个值，这样该元素就会继承其父元素的值。

  2.initial关键字
  每个css属性都有初始默认值，给属性设置initial会有效将其重置为默认值，比如给`<body>`设置color: #111; 那么默认的其子元素文本颜色都继承这个属性值，给其设置color: initial，会使其文本颜色变为
  color: black; 因为黑色是color属性的初始值。

  注：声明display: initial等价于display: inline。不管应用于哪种类型的元素，他都不会等于display:block。因为initial重置的是属性的初始值，而不是元素的初始值。inline才是display属性
  的初始值。

5. 简写属性

  简写属性是用于同时给多个属性赋值的属性。比如font是一个简写属性，还有：background,border,border-width

  1.简写属性会默默覆盖其他样式
  大多数简写可以省略一些值，只指定我们关注的值，但这样仍然会设置省略的值，即它们会被隐式的设置为初始值initial.这样会覆盖其他定义的样式

  2.理解简写值的顺序
  可以设置border: 1px solid black或者border: black 1pxsolid，两者都会生效。这是因为浏览器知道宽度、颜色、边框样式分别对应什么类型的值。但是有很多属性的值很模糊。在这种情况下，值的顺序很关键

  （1）上、右、下、左
    margin、padding、边框属性
  （2）水平、垂直
    上右下左只适合给盒子设置四个方向的值的属性。还有一些属性只支持最多指定两个值，这些属性包括background-position、box-shadow、text-shadow，比如box-shadow: 10px 2px #000;
    指定水平方向偏移量10px，垂直方向偏移量2px，先水平再垂直

### 第二章 相对单位

1.em
em的复杂之处在于同时用它指定一个元素的字号和其他属性。这时，浏览器必须先计算字号，然后使用这个计算值去算出其余的属性值。这两类属性可以拥有一样的声明值，但是计算值不一样。
例如：

```css
body {
  font-size: 16px;
}
.title {
  font-size: 1.2em; // 计算值为16*1.2=19.2px
  padding: 1.2em; // 计算值为19.2*1.2=23.04px
}
```

字体缩小的问题

```css
body {
  font-size: 16px;
}
ul {
  font-size: 0.8em;
}
```

```html
<ul> <!-- font-size: 16px * 0.8 -->
  <li>top level</li>
  <ul><!-- font-size: 16px * 0.8 * 0.8 -->
    <li>second level</li>
    <ul><!-- font-size: 16px * 0.8 * 0.8 * 0.8-->
      <li>third level</li>
    </ul>
  </ul>
</ul>
```

2.rem
在文档中，根节点是所有其他元素的祖先节点。根节点有一个伪类选择器（:root），可以用来选中它自己。这等价于类型选择器html，但是html的优先级相当于一个类名，而不是一个标签.
rem是root em的缩写。rem不是相对于当前元素，而是相对于根元素的单位.不管在文档的什么位置使用rem,1.2rem都会有相同的计算值：1.2乘以根元素的字号.

```css
:root {  /*  :root伪类相当于类型选择器html */
  font-size: 1em; /* 根元素上的em是相对于浏览器默认值的，16px  */
}
ul {
  font-size: 0.8rem;
}
```

拿不准的时候，用rem设置字号，用px设置边框，用em设置其他大部分属性.

em和rem都是相对于font-size定义的。

3.视口的相对单位
视口——浏览器窗口里网页可见部分的边框区域。它不包括浏览器的地址栏、工具栏、状态栏。

vh：视口高度的1/100。
vw：视口宽度的1/100。
vmin：视口高度的1/100。❑ vw：视口宽度的1/100。
vmax：视口宽、高中较大的一方的1/100。

当一个元素的宽和高为90vmin时，不管视口的大小或者方向是什么，总会显示成一个稍小于视口的正方形

vw可以结合calc()设置字号，对于iphone6 375到1200px，字号从11.75到20px

```css
:root {
  font-size: calc(0.5em + 1vw);
}
```

4.自定义属性即css变量

```css
:root {
  --main-foot: Arial, sans-serif;
}
p {
  font-family: var(--main-foot);
}
```

var()函数接受第二个参数，它指定了备用值。如果第一个参数指定的变量未定义，那么就会使用第二个值

如果var()函数算出来的是一个非法值，对应的属性就会设置为其初始值。比如，如果在padding: var(--brand-color)中的变量算出来是一个颜色，它就是一个非法的内边距值。这种情况下，内边距会设置为0.

自定义属性就像作用域变量一样,自定义属性就像作用域变量一样

```css
:root {
  --main-color: black;
}
.panel {
  --main-color: white
}
p {
  color: var(--main-color);
}
```

```html
<body>
  <p>黑色</p>
  <div class="panel">
    <p>白色</p>
  </div>
</body>
```

使用js改变属性：

```js
var rootElement = document.documentElement;
rootElement.style.setProperty('--main-color', '#ccc');
```

### 第3章 盒模型

1.盒模型
box-sizing: content-box; 设置元素宽高只设置内容盒子大小
box-sizing: border-box; width和height属性会设置内容、内边距及边框的大小总和。

```root {
  box-sizing: border-box;
}
*,
::before,
::after {
  box-sizing: .inherit;
}
```

盒模型通常不会被继承，但是使用inherit关键字可以强制继承.这样可以不覆盖第三方组件的盒模型