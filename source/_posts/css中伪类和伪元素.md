---
title: css中伪类和伪元素
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-03-28 14:49:23
---

## 伪类和伪元素

css引入伪类和伪元素是为了格式化文档树以外的信息。也就是说伪类和伪元素是为了修饰不在文档树中的部分。比如一句话中第一个字母或者列表的第一个元素。

### 伪类

伪类用于当已有元素处于某个状态时，为其添加的样式，这个状态是根据用户行为而动态变化的。比如用户鼠标悬停:hover

### 伪元素

伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如添加::before伪元素在一个元素前增加一些文本。并为这些文本添加样式。虽然这些文本用户可以看到，但并不存在文档树中。

伪类的操作对象是文档树中已有的元素，而伪元素则创建了一个文档树之外的元素。
因此，伪类与伪元素的区别在于：有没有创建一个文档树之外的元素。

### 伪元素是使用单冒号还是双冒号

虽然CSS3标准要求伪元素使用双冒号的写法，但也依然支持单冒号的写法。为了向后兼容，我们建议你在目前还是使用单冒号的写法。

### 伪类和伪元素的具体用法

伪类：

+ 状态：:link :visited :hover :actived :focus
+ 语言相关：:lang :dir
+ 其他：:root  :fullscreen
+ 表单相关：:checked :disabled :empty :enabled :valid :invalid :required :read-only :scope
+ 结构化：:not :first-child :last-child :first-of-type :last-of-type :nth-child :nth-of-child :only-child :only-of-type :target

伪元素：

+ 单双冒号：::before/:before   ::after/:after  ::first-letter/:first-letter  ::first-line/:first-line
+ 双冒号：::selection  ::placeholder  ::backdrop