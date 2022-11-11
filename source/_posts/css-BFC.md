---
title: css-BFC块级格式化上下文
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-14 17:57:42
---

### BOX： css布局的基本单位&盒模型

+ 盒模型--块级盒/行内盒
一个盒包含：内容（content）/边（border）/内边距（padding）/外边距（margin）
盒的尺寸（width和height--计算得到offsetWidth和offsetHeight）定义受到box-sizing属性的影响
w3c标准盒模型-块级盒-border-box：width = content + padding + margin
IE盒子模型-块级盒-content-box：width = content

+ 行内盒：
  + width/height 不起作用，盒子高度由内容决定（font-size/line-height）
  + margin-top/margin-bottom padding-top/padding-bottom不起作用

tips：

+ 两类块级盒子可用过设置box-sizing转换
+ 行内盒与块级盒转换可通过设置display属性来修改
+ 行内盒参与IFC布局，块级盒参与BFC布局，如果块级盒包含行内盒，但是由于BFC内只有块级盒参与，因此行内盒会被匿名块级盒包含

### BOX

Box 是 CSS 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 Box 组成的。元素的类型和 display 属性，决定了这个 Box 的类型。
不同类型的 Box， 会参与不同的 Formatting Context（一个决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染

+ block-level：box的display 属性为block, list-item, table 的元素，会生成 block-level box。并且参与（BFC）block fomatting context；
+ inline-level：box的display 属性为inline, inline-block, inline-table 的元素，会生成 inline-level box。并且参与 inline formatting context；

### 格式化上下文

+ 块级格式化上下文( Block formatting contexts )( BFC )
+ 行内格式化上下文( Inline formatting contexts ) ( IFC )
+ 自适应格式化上下文( Flex Formatting Contexts )( FFC )
+ 网格布局格式化上下文( GridLayout Formatting Contexts )( GFC )

有一类盒被称为块容器，它们能够包含块级盒。块容器要么创建BFC，这样它内部仅仅包含块级盒，要么创建一个IFC，这样它内部仅仅包含行内级元素。

（也就是说，块容器中不可能既包含块级盒，又包含行内级盒，一旦他的子盒中有块级盒，所有行内级盒都会被自动创建匿名盒包裹）。

在非块级格式化上下文中的块容器总是会创建新的BFC：如display为inline-blocks, table-cells, 和table-captions所生成的盒。

而自身也在块级格式化上下文中的块容器，则只有overflow不为visible的情形下才会创建新的BFC

+ 绝对定位和浮动的块容器则总是会创建新的块级格式化上下文。
+ display值为table或者inline-table的元素将会生成表格（table），表格内部会使用特殊的格式化方式来排布其内部元素。
+ display值为grid或者inline-grid的元素将会生成格元素（grid element），与table情形类似，它内部也是使用特殊的格式化方式来排布其内部元素，
+ display值为flex或者inline-flex的元素将会生成自适应容器（flex container），自适应容器在其内部产生自适应格式化上下文（flex formatting context）

FC的全称是：Formatting Contexts，是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

BFC:BFC(Block Formatting Contexts)直译为"块级格式化上下文"。
Block Formatting Contexts就是页面上的一个隔离的渲染区域，容器里面的子元素不会在布局上影响到外面的元素，反之也是如此。
浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。

BFC布局规则：

+ 内部的Box会在垂直方向，一个接一个地放置。
+ Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
+ 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
+ BFC的区域不会与float box重叠。
+ BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
+ 计算BFC的高度时，浮动元素也参与计算

那些元素生成BFC

+ 根元素
+ float属性不为none
+ position为absolute或fixed
+ display为inline-block, table-cell, table-caption, flex, inline-flex
+ overflow不为visible

元素与盒

+ 在HTML中常常使用的概念是元素，而在CSS中，布局的基本单位是盒，盒总是矩形的。
+ 元素与盒并非一一对应的关系，一个元素可能生成多个盒，CSS规则中的伪元素也可能生成盒，display属性为none的元素则不生成盒。
+ 除了元素之外，HTML中的文本节点也可能会生成盒。

正常流

+ 正常流是页面，大部分盒排布于正常流中。正常流中的盒必定位于某一格式化上下文中，正常流中有两种格式化上下文：块级格式化上下文（block formatting context，简称BFC）和行内格式化上下文（inline formatting context,IFC）。
+ 在块级格式化上下文中，盒呈纵向排布，在行内格式化上下文中，盒则呈横向排布。
+ 正常流根容器中是块级格式化上下文，不同的盒可能会在内部产生行内格式化上下文或者块级格式化上下文。

块级与行内级

+ 正常流中的盒分为块级与行内级两种，任何一个行内级盒都不能够直接被放入块级格式化上下文中。如果有一个HTML元素生成了一个行内盒，而其所在的上下文是块级的话，那么应当为它生成一个匿名块级盒，匿名块级盒会在内部生成行内格式化上下文。

+ 元素的display属性会决定盒是行内级还是块级：
block, table, flex, grid, list-item 为块级
inline, inline-block, inline-table, inline-flex, inline-grid 为行内级

产生垂直外边距合并的必备条件,两个margin是邻接的必须满足以下条件:

+ 必须是处于常规文档流（非float和绝对定位）的块级盒子,并且处于同一个BFC当中
没有线盒，没有空隙（clearance，下面会讲到），没有padding和border将他们分隔开,都属于垂直方向上相邻的外边距，可以是下面任意一种情况:
  + 元素的margin-top与其第一个常规文档流的子元素的margin-top
  + 元素的margin-bottom与其下一个常规文档流的兄弟元素的margin-top
  + height为auto的元素的margin-bottom与其最后一个常规文档流的子元素的margin-bottom
  + 高度为0并且最小高度也为0，不包含常规文档流的子元素，并且自身没有建立新的BFC的元素的margin-top和margin-bottom
