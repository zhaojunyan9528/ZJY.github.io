---
title: css-css3选择器
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-19 16:44:55
---

### 类型选择器： 标签名进行选择

+ h1{}
+ h1,h2{} 选择器群组

### 通用选择器： *

### 类选择器： 

+ .content{}

### ID选择器

+ #title{}

### 属性选择器： 

+ [attribute]
+ [attribute1][attribute2]
+ [attribute = value]
+ [attribute ~= value] 选择器用于选取属性值中包含指定词汇的元素 [title ~= 'flower']
+ [attribute |= value] 选择器用于选取带有以指定值开头的属性值的元素 [leng | en]
+ [attribute ^= value] 以value开头的值 [class ^="test"]
+ [attribute $= value] 以value结尾的值
+ [attribute *= value] 选择器匹配属性值包含指定值的每个元素

### 伪类：

+ a:link a:visited a:hover a:active :focus 
+ :target 选择器可用于选取当前活动的目标元素
+ :disabled 用于添加禁用样式

+ :nth-child(n) 选择器匹配属于其父元素的第 N 个子元素，不论元素的类
+ :nth-last-child() 同上，从最后一个子元素开始计数
+ :first-child  p:first-child 选择属于父元素的第一个子元素的每个``<p>``元素
+ :last-child  p:last-child	选择属于其父元素最后一个子元素每个`<p>`元素。

+ :nth-of-type() p:nth-of-type(2) 选择属于其父元素第二个`<p>`元素的每个`<p>`元素
+ :nth-last-of-type() 同上，但是从最后一个子元素开始计数
+ :first-of-type  p:first-of-type	选择属于其父元素的首个`<p>`元素的每个`<p>`元素
+ :last-of-type  p:last-of-type	选择属于其父元素的最后`<p>`元素的每个`<p>`元素
+ 
+ :only-child  p:only-child	选择属于其父元素的唯一子元素的每个`<p>`元素。
+ :only-of-type  p:only-of-type	选择属于其父元素唯一的`<p>`元素的每个`<p>`元素

+ :empty p:empty	选择没有子元素的每个`<p>`元素（包括文本节点）

+ :not :not(p)	选择非`<p>`元素的每个元素。

### 伪元素选择器：

+ ::first-line p::first-line	选择每个`<p>`元素的首行。
+ ::first-letter	p::first-letter	选择每个`<p>`元素的首字母。

+ ::before	p::before	在每个`<p>`元素的内容之前插入内容。
+ ::after	p::after	在每个`<p>`元素的内容之后插入内容。

### 组合选择器： 

+ div img  以空格隔开 后裔选择器

### 儿子选择器：

+ div > img

### 兄弟选择器：

+ div + img
+ h2 ~ h3 h2 后面的h3 元素