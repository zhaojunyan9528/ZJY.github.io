---
title: javascript的Object.create方法
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-01 17:19:59
---
##  Object.create()

Object.create()方法创建一个新的对象,使用现有的对象来提供新创建对象的__proto__.

语法:
Object.create(proto, [propertiesObject])
参数:
proto:新创建对象的原型对象,
propertiesObject:可选,需要传入一个对象.该对象的属性类型,参照Object.defineProperties()的,
第二个参数.如果该参数被指定且不为undefined,该传入对象的自有可枚举属性(即其自身定义的属性,而不
是其原型链上的枚举属性)将为新创建的对象添加指定的属性值和对应的属性描述符.
返回值:一个新对象,带着指定的原型对象和属性
例外:如果propertiesObject参数是null或非原始包装对象,则抛出一个typeerror异常

```js
const person = {
  isHuman: false,
  printIntroduction: function() {
      console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};
const me = Object.create(person);
console.log(me);
// me{
//     __proto__:{
//         isHuman: false,
//         printIntroduction:function()
//     }
// }
me.name = 'zjy';
me.isHuman = true;
me.printIntroduction();
console.log(me);
// me{
//     isHuman: true,
//     name:'zjy',
//     __proto__:{
//         isHuman: false,
//         printIntroduction:function()
//     }
// }



var xxx = Object.create({
    a:'a'
},{
    'str':{
        value:'zjy'
    },
    'name':{
        value:'ha'
    }
});
console.log(xxx);
// xxx{
//     name:'ha',
//     xxx:'zjy',
//     __proto__:{
//         a:'a'
//     }
// }



var aObj = {
    a:'zjy',
    b:'name',
    c:function(n){
        console.log('n:'+n);
    }
};
var _aObj = Object.create(aObj,{
    txt:{
        value:'Object.create()方法的继承'
    }
});
console.log(_aObj);
// _aObj{
//     txt:'Object.create()方法的继承',
//     __proto__:{
//         a:'zjy',
//         b:'name',
//         c:funtion()
//     }
// }
_aObj.c('haha');//n:haha
```