---
title: window.sessionStorage的理解
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-04-10 10:43:47
---

sessionStorage 属性允许你访问一个，对应当前源的 session Storage 对象。它与 localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除

+ 页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。
+ 在新标签或窗口打开一个页面时会复制顶级浏览会话的上下文作为新会话的上下文，这点和 session cookies 的运行方式不同。
  使用target="_blank"的A标签，window.open打开同源的新窗口时，会把旧窗口的sessionStorage数据带过去，但新旧窗口sessionStorage数据不同步
+ 打开多个相同的URL的Tabs页面，会创建各自的sessionStorage。
+ 关闭对应浏览器窗口（Window）/ tab，会清除对应的sessionStorage