---
title: CSS3新特性
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-02-22 17:13:22
---
## 边框属性

### 1.border-image边框图片属性

border-image:none 100% 1 0 stretch; 默认值
border-image是一个简写，用于设置以下属性：

+ border-image-source: 边框图片路径
+ border-image-slice: 图片边框向内偏移
+ border-image-width: 图片边框的宽度
+ border-image-outset: 边框图像区域超出边框的量
+ border-image-repeat: 图片边框是否平铺（repeated）,铺满（rounded）或拉伸（stretched）

### 2.border-radius边框圆角属性

border-radius: 0; 默认值
border-radius是一个简写，用于设置以下属性：

+ border-top-left-radius: 左上角圆角属性
+ border-top-right-radius: 右上角圆角属性，若省略则取top-left的值
+ border-bottom-left-radius: 左下角圆角属性，若省略则取top-right的值
+ border-bottom-right-radius: 右下角圆角属性，若省略则取top-left的值

以上属性值可取2种值：

+ length: 具体的值,比如2px,2em等
+ %: 百分比

### 3.box-shadow边框阴影属性

box-shadow属性向边框添加一个或多个阴影，是由逗号分隔的阴影列表，每个阴影由2-4个长度值、可选的颜色以及可选的inset关键词来规定。省略长度的值是0.

box-shadow属性值：

+ h-shadow: 必需，水平阴影的位置，允许负值
+ v-shadow：必需，垂直阴影的位置，允许负值
+ blur： 可选，模糊距离
+ spread：可选，阴影的尺寸
+ color：可选，阴影的颜色
+ inset：可选，将外部阴影设置为内部阴影

## 背景属性

### 1.background-image多背景图片属性

background-image:url(bg_flower.gif),url(bg_flower_2.gif);

### 2.background-clip背景的绘制区域

background-clip 属性规定背景的绘制区域

background-clip属性可选值：

+ border-box：默认值，图片被剪裁到边框盒
+ padding-box: 图片被剪裁到内边距框
+ content-box: 图片被剪裁到内容框

### 3.background-origin规定背景图片的定位区域

background-origin属性规定background-position属性相对于什么位置来定位。
如果background-attachment属性设置为“fixed”，该属性无效。

background-origin属性值：

+ padding-box: 默认值，背景图片相对于内边距框来定位
+ border-box: 背景图片相对于边框盒来定位
+ content-box: 背景图片相对于内容框来定位

### background-size规定背景图片的尺寸

background-size 属性规定背景图像的尺寸

background-size: auto;默认值

background-size取值四种方式：

+ length：设置宽度和高度，第一个设置宽度，第二个值设置高度，如果只设置一个值，第二个值会被设置为"auto".
+ %:百分比设置宽度和高度，相对于父元素的宽高设置，第一个设置宽度，第二个值设置高度，如果只设置一个值，第二个值会被设置为"auto".
+ cover：把背景图片扩展至足够大，以使背景图像完全覆盖背景区域
+ contain：把背景图片扩展至最大尺寸，以使其宽高完全适应内容区域

## 文本效果

### 1.text-shadow文本阴影

text-shadow 属性向文本设置阴影。
text-shadow: none;默认值

text-shadow属性包含下列值：

+ h-shadow: 必需，水平阴影的位置，允许负值
+ v-shadow: 必需，垂直阴影的位置，允许负值
+ blur: 可选，模糊的距离
+ color: 可选，阴影的颜色
  
### 2.word-wrap属性

word-wrap属性允许长单词或 URL 地址换行到下一行。

word-wrap:normal;默认值

+ break-word：允许长单词或url地址换到下一行
+ normal: 只在允许的断字点换行（浏览器保持默认处理）

### 3.word-break属性规定自动换行的处理方法

word-break: noraml默认值

+ noraml:浏览器默认换行规则
+ break-all: 允许在单词内换行
+ keep-all： 只允许在半角空格或连字符处换行

## transform属性

transform属性向元素应用2D或3D属性。该属性允许元素进行旋转、缩放、倾斜、移动

transform:none/transform-function,none为默认值

transform值可以是以下方法：

+ none：不进行任何转换
+ matrix(n,n,n,n,n,n):定义2D转换，使用六个值的矩阵
+ matrix3d(n,...n):定义3D转换，使用16个值的矩阵
+ translate(x,y):定义2D转换
+ translate3d(x,y,z): 定义3D转换
+ translateX(x): 定义转换，只是用 X 轴的值。
+ translateY(y): 定义转换，只是用 Y 轴的值。
+ translateZ(z): 定义转换，只是用 Z 轴的值。
+ scale(x,y): 定义2D缩放转换
+ scale3d(x,y,z): 定义3D缩放转换
+ scaleX(x): 通过设置 X 轴的值来定义缩放转换。
+ scaleY(y): 通过设置 Y 轴的值来定义缩放转换。
+ scaleZ(z): 通过设置 Z 轴的值来定义缩放转换。
+ rotate(angle): 定义 2D 旋转，在参数中规定角度
+ rotate(x,y,angle): 定义3D旋转
+ rotateX(angle): 定义沿着 X 轴的 3D 旋转
+ rotatezY(angle): 定义沿着 Y 轴的 3D 旋转
+ rotateZ(angle): 定义沿着 Z 轴的 3D 旋转
+ skew(x-angle,y-angle): 定义沿着x轴和y轴倾斜转换
+ skewX(angle): 定义沿着X轴的2D倾斜转换
+ skewY(angle):定义沿着y轴的2D倾斜转换

## transition过渡属性

transition过渡是元素从一种样式逐渐改变为另一种效果，需要规定2项内容：

+ 规定希望效果作用到元素哪个属性上
+ 规定效果的时长

transition是简写属性，包含以下转换属性：

+ transition-property:规定应用过渡的css属性名称，取值：none/property/all
+ transition-duration:规定应用过渡时长，默认值0，以秒或毫秒计
+ transition-timing-function: 规定过渡效果的曲线，默认值是ease
+ transition-delay: 规定效果从何时开始，默认值是0

```css
div
{
  transition: width 2s;
  -moz-transition: width 2s;	/* Firefox 4 */
  -webkit-transition: width 2s;	/* Safari 和 Chrome */
  -o-transition: width 2s;	/* Opera */
}
div:hover{
  width: 300px;
}
```

## animation动画

@keyframes规则用于创建动画

```css
@keyframes myFirst{
  from {
    background-color: red;
  }
  to {
    background-color: yellow;
  }
}
```

在@keyframes创建动画后，需要将它绑定到某个选择器，需要以下2项css3动画属性，即可将动画绑定到选择器：

+ 规定动画的名称
+ 规定动画的时长

```css
div{
  animation: myFirst 5s;
}
```

可用0%和100%代替from和to。

+ @keyframes：创建动画
+ animation：动画属性简写
+ animation-name: 规定@keyframes动画的名称
+ animation-duration: 规定动画的一个周期所需时长，默认是0
+ animation-timing-function: 规定动画的曲线，默认是ease
+ animation-delay: 规定动画何时开始，默认是0
+ animation-iteration-count: 规定动画播放次数，默认是1，值可取n/infinate(无限播放)
+ animation-direction: 规定动画是否在下一周期逆向播放，默认是normal,取值normal/alternate
+ animation-play-state: 规定动画是正在运行还是暂停，默认值是running，取值running/paused
+ animation-fill-mode: 定对象动画时间之外的状态

## 弹性盒子flexbox

## 多媒体查询@media

