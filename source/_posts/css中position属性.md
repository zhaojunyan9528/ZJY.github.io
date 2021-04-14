---
title: css中position属性
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-04-14 20:59:55
---

## position

position属性用于指定一个元素在文档中定位方式。top,left,right,bottom属性则决定了元素的最终位置。

### 1.定位类型

定位元素是定位为relative,absolute,fixed,sticky的一个元素，也就是除static外的定位元素。

相对定位：relative
绝对定位：absolute/fixed
粘性定位：sticky

如果top和bottom都指定了，top优先
如果left和right都指定了，当direction设置为ltr时，left优先，当directin设置rtl时，right优先。

### 2.取值

<b>static</b>
该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 top, right, bottom, left 和 z-index 属性无效。

<b>relative</b>
该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。

相对定位的元素是在文档中的正常位置偏移给定的值，但是不影响其他元素的偏移。

<b>absolute</b>
元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非static定位的父元素进行偏移，来确定元素的位置。绝对定位元素可以设置margin外边距且不会与其他边距合并。

相对定位的元素并未脱离文档流，而绝对定位的元素则脱离了文档流。在布置文档流中其它元素时，绝对定位元素不占据空间。绝对定位元素相对于最近的非 static 祖先元素定位。当这样的祖先元素不存在时，则相对于ICB（inital container block, 初始包含块）

<b>fixed</b>
元素会被移出正常文档流，并不为元素预留空间。通过指定元素相对于视窗来进行定位，元素的位置在页面滚动时不会改变。打印时，元素出现在每页的固定位置。fixed会创建新的层叠上下文，当元素祖先的transform,perspective或filter属性不为none时，容器定位由视窗变为该祖先。

固定定位与绝对定位相似，但元素的包含块为 viewport 视口

<b>sticky</b>
元素根据正常文档流进行定位，然后相对它的最近滚动祖先和最近块级祖先，包括table-related元素，基于top, right, bottom, 和 left的值进行偏移。偏移值不会影响任何其他元素的位置。该值总是创建一个新的层叠上下文（stacking context）

粘性定位可以被认为是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。

#one { position: sticky; top: 10px; }

在 viewport 视口滚动到元素 top 距离小于 10px 之前，元素为相对定位。之后，元素将固定在与顶部距离 10px 的位置，直到 viewport 视口回滚到阈值以下

粘性定位常用于定位字母列表的头部元素。标示 B 部分开始的头部元素在滚动 A 部分时，始终处于 A 的下方。而在开始滚动 B 部分时，B 的头部会固定在屏幕顶部，直到所有 B 的项均完成滚动后，才被 C 的头部替代。

须指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。