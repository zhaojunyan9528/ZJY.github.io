---
title: 记element table组件固定高度滑动底部数据渲染问题
tags:
  - 前端
categories:
  - - 问题&总结
    - vue
date: 2021-12-17 09:11:56
---

### 1.问题

element中table组件,设置固定高度height后,数据渲染超出设置高度后,鼠标向下滑动,此时包裹table元素`<div class="el-table__body-wrapper is-scrolling-left/right/middle/none">`scrollTop值为滚动距离,若此时切换到第二页,数据渲染未超出设置height,再切换到第一页,数据渲染超出设置高度后,此时table应有滚动条,但是table组件只渲染部分数据,没有出现滚动条,固定高度区域仍有空白.

### 2.原因

该元素`<div class="el-table__body-wrapper is-scrolling-left/right/middle/none">`scrollTop值仍为上一次滚动距离,没有重新归0,导致table只渲染部分数据,表格固定高度区域有空白,需要鼠标向下滚动才会重新渲染剩余数据

### 3.解决

每次数据改变时,判断该元素`<div class="el-table__body-wrapper is-scrolling-left/right/middle/none">`scrollTop值不为0时,强制归0.

给`<el-table ref="baseTable" />`组件设置ref属性值
打印$refs.baseTable组件,其$refs下有bodyWrapper组件,处理该组件scrollTop值即可
this.$refs.baseTable.$refs.bodyWrapper.scrollTop = 0