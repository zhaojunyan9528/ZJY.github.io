---
title: target和currentTarget区别
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-19 22:03:31
---

## target和currentTarget区别

```html
<ul id="ulT">
    <li class="item1">1</li>
    <li class="item2">2</li>
    <li class="item3">3</li>
    <li class="item4">4</li>
    <li class="item5">5</li>
</ul>
```

此时target和currentTarget是一样的 都是点击的li标签

```javascript
var lis = document.querySelectorAll('li');
for(var i =0;i<lis.length;i++){
    lis[i].onclick= function (e) {
        console.log(e.target);  //<li class="item1">1</li>
        console.log(e.currentTarget); //<li class="item1">1</li>
    }
}
```

此时是不一样的

```javascript
var ul = document.querySelector('ul');
ul.addEventListener('click', function (e) {
    console.log(e.target);  //当前的li
    console.log(e.currentTarget); //元素的ul
})
```