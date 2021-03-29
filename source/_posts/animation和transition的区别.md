---
title: animation和transition的区别
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-03-23 17:23:04
---

## animation和transition的区别

### css3动画和js动画的区别

js实现的是帧动画，css3实现的是补间动画
帧动画：使用定时器，每隔一段时间就更改元素
js动画逐帧绘制内容，可操作性高，可以很好的控制动画开始，暂停，回放，取帧等；但复杂度高于css动画，资源占有比较大
补间动画：状态发生变化产生动画
只需要确定第一帧和最后一帧的位置，2个关键帧之间内容由composite线程自动生成，不需要人为处理。对动画控制比较弱。



### transition

transition属性设置过度属性，四个简写属性为：

+ transition-property:指定css属性的name，默认是all
+ transition-duration：transition效果需要指定多少秒或毫秒才能完成，默认为0
+ transition-timing-function:指定transition效果的转速曲线,默认值ease
  + linear：规定以相同的速度开始至结束的过度效果
  + ease：规定慢速开始，然后变快，然后慢速结束的过渡效果
  + ease-in：规定以慢速开始的过渡效果
  + ease-in-out:规定以慢速开始和结束的过渡效果
+ transition-delay:定义transition效果延迟多久开始

transition需要事件触发（比如hover类），不能在加载网页时自动开始
一次性，不能重复触发
只有2个状态，开始和结束
一个transition属性只能定义一个属性

### animation

animation属性用来创建动画

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

@keyframes创建动画后，需要将它绑定到某个选择器，需要一下2个css属性，才能将动画绑定到选择器：
1.规定动画的名称
2.规定动画的时长

```css
div{
  animation: myFirst 5s;
}
```

可用0%到100%代替from和to

+ @keyframes：创建动画
+ animation：动画的简写，默认值none 0 ease 0 1 normal
+ animation-name: 规定@keyframes动画的名称
+ animation-duration: 规定动画的一个周期所需时长，默认是0
+ animation-timing-function: 规定动画的曲线，默认是ease
+ animation-delay: 规定动画何时开始，默认是0
+ animation-iteration-count: 规定动画播放次数，默认是1，值可取n/infinate(无限播放)
+ animation-direction: 规定动画是否在下一周期逆向播放，默认是normal,取值normal/alternate
+ animation-play-state: 规定动画是正在运行还是暂停，默认值是running，取值running/paused
+ animation-fill-mode: 定对象动画时间之外的状态

总结：

|区别|transition|animation|
|:---:|:---:|:---:|
|能否自动执行|不能，需要事件触发，比如hover|能|
|能否重复发生|不能，除非重复触发|能|
|是否有多个状态|2个，开始和结束状态|能，比如从0% 到100%，任意指定过渡状态|
|能否暂停|不能|能|
|能否定义速度曲线|能|能|
|能否定义某个属性值过渡|能|能|