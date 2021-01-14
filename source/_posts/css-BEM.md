---
title: css-BEM
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-14 17:50:50
---

BlockElementModifier其实是块（block）、元素（element）、修饰符（modifier）。
这三个部分使用__ 与`--`连接:
.块__元素--修饰符{}

block 代表了更高级别的抽象或组件
block__element 代表 block 的后代，用于形成一个完整的 block 的整体
block`--`modifier代表 block 的不同状态或不同版本

css引擎查找样式表，对每条规则都按从右到做的顺序去匹配
在scss中使用：使用@at-root内联选择器模式，编译出来的CSS无任何嵌套（这是关键）

```css
.person {
  @at-root #{&}__hand {
    color: red;
    @at-root #{&}--left {
     color: yellow;
    }
  }
  @at-root #{&}--female {
    color: blue;
    @at-root #{&}__hand {
      color: green;
    }
  }
}
/*生成的css*/
.person__hand {
   color: red;
}
.person__hand--left {
   color: yellow; 
}
.person--female{
  color: blue;
}
.person--female__hand {
  color: green;
}
```