---
title: css中的z-index属性
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-04-11 16:03:30
---

### 1.z-index基础概念

通过设置定位及 top，left，bottom 和 right 的值，你可以在二维空间中对元素进行定位，但 CSS 同时也允许你使用 z-index 属性把它放置在三维空间中。

x 轴代表水平方向，y 轴代表垂直方向，z 轴则代表我们的目光向页面（屏幕）看进去的时候，各元素的布局情况

为了决定某个元素在 z 轴方向上的位置，CSS 允许我们为 z-index 属性设置三种值：

+ auto默认值
+ 整数
+ inherit

看整数值：它可以是正整数、负整数或者 0，值越大，元素就离我们“越近”，值越小，元素自然也就离我们“越远”

### 2.层叠上下文

每个网页都会默认创建一个层叠上下文，这个上下文的根部就是html元素，html元素的所有子元素都会默认在上下文中的某个层叠等级。

当你个某个元素设置z-index值非auto时，就会创建一个新的层叠上下文。它和它所包含的层叠等级都是独立于其他层叠上下文和层叠等级的。

在一个层叠上下文中，一共可能出现七个层叠等级，从低到高排序依次是：

+ 1.背景和边框：形成层叠上下文的元素的背景和边框，它是整个上下文中层叠等级最低的。
+ 2.z-index为负数：设置了 z-index 为负数的子元素以及由它所产生的层叠上下文
+ 3.块级盒模型：位于正常文档流中的、块级的、非定位的子元素
+ 4.浮动盒模型：浮动的、非定位的子元素
+ 5.内联盒模型：位于正常文档流中的、内联的、非定位的子元素
+ 6.z-index为0：设置了 z-index 为 0 的、定位的子元素以及由它所产生的层叠上下文
+ 7.z-index为正数:设置了 z-index 为正数的、定位的子元素以及由它所产生的层叠上下文，它是整个上下文中层叠等级最高的

大部分元素的层级都要低于 z-index:0。

例子：

```html
<div class="one">
  <div class="two"></div>
  <div class="three"></div>
</div>
<div class="four"></div>
```

```css
div {
  width: 200px;
  height: 200px;
  padding: 20px;
}
 
.one, .two, .three, .four {
  position: absolute;
}
  
.one {
  background: #f00;
  outline: 5px solid #000;
  top: 100px;
  left: 200px;
  z-index: 10;
}
  
.two {
  background: #0f0;
  outline: 5px solid #000;
  top: 50px;
  left: 75px;
  z-index: 100;
}
 
.three {
  background: #0ff;
  outline: 5px solid #000;
  top: 125px;
  left: 25px;
  z-index: 150;
}
 
.four {
  background: #00f;
  outline: 5px solid #ff0;
  top: 200px;
  left: 350px;
  z-index: 50;
}
```

尽管 div.two 有更高的 z-index（100），但在页面上，它的层级实际上比 div.four （z-index 为50）要低
由于 div.two  位于 div.one 中，所以它的 z-index 是和 div.one 的层叠上下文相关的，也就是说，实际表现出来的 z-index 是下面这样的：

+ .one —— z-index = 10
+ .two —— z-index = 10.100
+ .three —— z-index = 10.150
+ .four —— z-index = 50

### 3.总结

定位元素可以创建新的层叠上下文，在这个上下文中的所有层叠等级，都会高于或者低于另一个层叠上下文的所有层叠等级

