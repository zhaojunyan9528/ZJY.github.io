---
title: css-background属性使用
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-14 17:24:41
---

## Background

Background 是一种 CSS 简写属性，一次性定义了所有的背景属性，包括 color, image, origin 还有 size, repeat 方式等等。

语法：

+ background: background-color，background-image，background-repeat，background-attachment，background-position;(不强制要求书写顺序)

    + background-color 指定要使用的背景颜色 transparent 

    + background-position 指定背景图像的位置 0%, 0% 

    + background-image 指定要使用的一个或多个背景图像 none 

    + background-repeat 指定如何重复背景图像 repeat

    + background-attachment 设置背景图像是否固定或者随着页面的其余部分滚动。 croll

    + background-size 指定背景图片的大小 auto CSS3

    + background-origin 指定背景图像的定位区域 padding-box CSS3

    + background-clip 指定背景图像的绘画区域 border-box CSS3

多背景图片 background-image：

```javascript
    background-image: url('img1'), url('img2');
    background-size: 50%, 100%;
    background-repeat: repeat-x, no-repeat;
```

多背景图片总结：

+ 背景图片所生效的样式，是属性值中与图片位置对应的值；
+ 如果属性值比背景图片的个数要少，那么没有对应的值的图片样式以第一个值为准；
+ 背景图片的层级按着从左往右，依次减小。当然，层级最低的还是 background-color；

背景渐变 background-image: linear-gradient：路径渐变（可手动设置方向，默认自下向上）

```javascript
    background: linear-gradient(to left, #333, #333 50%, #eee 75%, #333 75%);
    background-image: linear-gradient(#71c9ce, #e3fdfd);
```

background-image: radial-gradient 径向渐变：

```javascript
    background-image: radial-gradient( #71c9ce, #e3fdfd);
```

background-image: repeating-linear-gradient 重复路径渐变：

```javascript
    background-image: repeating-linear-gradient(45deg, #71c9ce 20px, #a6e3e9 30px, #e3fdfd 40px);
```

background-image: repeating-radial-gradient 重复径向渐变：

```javascript
    background-image: repeating-radial-gradient(circle, #90ade4 ,#3350ba 20%);
```

背景定位 background-position：

三个盒子：
+ border-box  即所设置元素的 border 所占的区域，位于 padding 和 content 的外层
+ padding-box  即所设置元素的 padding 所占的区域，位于 border的内层、content 的外层
+ content-box 元素的 padding 所占区域包围着的即为 content

background-position 默认的定位为 padding-box 盒子的左上角。其属性值可设置为：百分比 / 像素 /位置

背景重复 background-repeat：

除了常见的几个 repeat、repeat-x，repeat-y 以及 no-repeat 以外，还在CSS3 中新加了两个值： space 和 round：
+ 1.背景图片小于容器时
    + background-repeat:space 在保证不缩放的前提下尽可能多的重复图片，并等分图片中间的空隙
    + background-repeat:round 在尽可能多的重复图片的前提下，拉伸图片以铺满容器
+ 2.背景图片大于容器时
    + background-repeat:space 在不缩放的前提下裁剪图片，只保留在容器内的部分
    + background-repeat:round 缩小图片以铺满容器，长宽与容器尺寸一致（未按比例缩放，图片极有可能变形）

背景相对位置 background-origin：

+ background-origin 属性规定 background-position 属性相对于什么位置来定位。属性值有 content-box 、padding-box 、border-box 三个，默认为 padding-box

背景绘制区域 background-clip：

+ background-clip 属性规定背景的绘制区域。默认值为 border-box，其属性值同 background-origin 一样，不过表现大不相同

背景大小 background-size：

+ background-size 除了常见的设置大小和百分比之外，还有两个特殊的属性：contain 和 cover
+ background-size: contain 图片长宽不相同时，把图片按比例缩小至较长的一方完全适应内容区域为止，多用于背景图片比元素大的情况
+ background-size: cover 图片长宽不相同时，把图片按比例放大至较短的一方完全适应内容区域为止，以使背景图像完全覆盖背景区域，多用于背景图片比元素小的情况。

背景固定 background-attachment：fixed 背景固定/scroll 背景随页面滚动而滚动（默认）

扩展属性 background: element

+ 一个特殊的扩展属性，可以将某个元素设置为另一元素的背景。惊不惊喜，意不意外！不过这个属性只有 FireFox 4+ 的浏览器可以使用，并且需要加上浏览器前缀。
    background: element(#id)