---
title: css-flex布局
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-19 16:49:28
---

### flexbox 布局

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为 Flex 布局

flex:包含两部分：flex-container容器 和 flex-item

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。

主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

### container属性:

    flex-direction
    flex-wrap
    flex-flow
    justify-content
    align-items
    align-content

1.display属性：

    flex
    inline-flex 行内容器
    -webkit-flex Webkit 内核的浏览器，必须加上-webkit前缀。
注意，设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效

2.flex-direction:

    row 水平方向从左向右
    column 垂直方向 从上往下
    row-reverse 水平方向从右向左
    column-reverse 垂直方向 从下向上

3.flex-wrap: 默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行

    nowrap 不换行 默认值
    wrap 换行,第一行在上方。
    wrap-reverse 换行，第一行在下方。

4.flex-flow: flex-direction 和 flex-wrap 结合

    row nowrap 两个参数对应

5.justify-content： 定义了项目在主轴上的对齐方式

    center
    space-between
    space-around
    space-evenly
    flex-start
    flex-end
<!-- ![avatar](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png) -->

6.align-item：属性定义项目在交叉轴上如何对齐

    stretch 拉伸 如果项目未设置高度或设为auto，将占满整个容器的高度。
    flex-start 交叉轴的起点对齐。
    flex-end 交叉轴的终点对齐。
    center 交叉轴的中点对齐
    baseline 项目的第一行文字的基线对齐

7.align-content: 多行显示设置垂直对齐方式 属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用

    stretch
    flex-start
    flex-end
    center
    space-between 行与行之间添加间隔
    space-around 行周围间隔

### item属性：

1.order: 控制item在容器里的位置 给item设置

    默认为0
    值越大排越后

2.flex-grow： 控制item的宽度 值越大，宽度越宽,定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

    flex-grow: 1; 

3.flex-basies: 设置item的宽度,定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

    flex-basies: 100px
    auto 默认值

4.flex-shrink: 设置item缩小的比例,默认为1，即如果空间不足，该项目将缩小

    flex-shrink: 0; 设置不缩小
    设置比1大的数可设置缩小比例更大些
    如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小

5.flex: 0 1 auto;分别对应 flex-grow flex-shrink flex-basies

6.align-self: 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

    align-self: auto/flex-start/flex-end/center/stretch/baseline 