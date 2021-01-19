---
title: css-css3动画
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-19 16:41:04
---

### 过渡动画transition:
	
	transition-property:动画属性：width height  border-radius ...
	transition-delay: 动画延迟时间
	transition-timing-function: 运动曲线函数
	transition-duration: 过渡时间
	transition: all 1s ease 0s
	
### transform:

	rotate:旋转
	translate:移动
	scale: 缩放
	skew: 倾斜
	
### 阴影：shadow

	box-shadow: 
		h-shadow水平阴影位置（必须） 
		v-shadow垂直阴影的位置（必须） 
		blur可选。模糊距离。 
		spread可选。阴影的尺寸。 
		color可选。阴影的颜色。
		inset可选。将外部阴影 (outset) 改为内部阴影。
		
### 渐变：linear-gradient:
	
	linear-gradient:
		direction:用角度值指定渐变的方向（或角度） top left...
		color-stop1,color-stop2,...
		
### animation:

	-webkit-animation-name: hover ; //名字
	-webkit-animation-duration: 1s; //时间
	-webkit-animation-timing-function: ease-in; //变速曲线
	-webkit-animation-iteration-count: infinite; //次数
	