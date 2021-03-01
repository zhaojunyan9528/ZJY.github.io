---
title: javascript中this的指向
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 14:13:51
---

## javascript中this的指向

```js
function xx(){
    console.log(this); //window
    console.log(this === window); //true
}
xx();
```

```js
var yy = {
  a:'zjy',
  b:function(){
      console.log(this);//yy
      console.log(this === yy);//true
      function bb(){
          console.log(this);//window
      }
      bb();//bb只是在yy的属性b中执行，并不是b的属性，属于window
  }
};
yy.b();
```

```js
var o = {
    s:function(){
        console.log(this);
    }
}
var f = o.s;
f();//this指向window,js中函数是按引用传递的
```

```js
function xx3(){
    this.x = 333;
    console.log(this); //xx3:{x: 333}
    console.log(this.__proto__==xx3.prototype);//true
}
new xx3();
```

