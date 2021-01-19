---
title: es6-模块化
tags:
  - 前端
categories:
  - - 笔记
    - es6
date: 2021-01-19 17:56:16
---

### es5中，用module.exports 和 exports导出模块，用require引入模块

### es6中，新增export 和 export default 导出模块，import引入模块

1. module.exports 和 exports 的区别：

module.exports 和 exports 导出的对象，可以在另一个文件中通过require引用

module 和 exports 是node.js给每个js文件内置的两个对象，初始值：

```javascript
console.log(exports):  {}  空对象
console.log(module): {..., exports: {} }  ...代表其他属性如：id,filename...
```

一开始这两个对象都是空对象{}，实际上这两个对象指向同一块内存
exports.age = 18 和 module.exports.age = 18 两者是等价的

但是require引入的本质是module.exports，所有当module.exports 和exports
指向的不是同一块内存时，exports的内容就会失效：例如

```javascript
a.js:
exports = {name:'lily'}
module.exports = {name:'lucy' }
b.js:
let test = require('./a')
consle.log(test)  // {name:'lucy'}
```

2. export default 和 export 的区别：

```javascript
const People = { name: 'lucy' ,age: 18 }
export default = People
console.log(module):
{exports:{default:{name:'lucy',age:18}}}
import Peole from '.a'

const a = { name:'aaa',age,12}
const b = { name:'bbb', age:23}
export { a, b}
console.log(module):
{exports: {a:{name:'aaa',age:12},b:{name:'bbb',age:23}}}
```

导入： import {a,b} from './a'

导出时， export 相当于将对象添加到module 的 exports中，
export default 相当于将对象添加到module 的 exports ，对象key 为default

导入时：
不带{}的导入，本质上就是导入exports的default属性，若default属性不存在，
则导入exports对象
带{}的导入，按key值导入exports中对应的属性值

一般来说，module.exports和exports与require对应。也就是用module.exports和exports
导出的模块，则用require导入。（不是绝对，如果代码支持es6，也可以用import引入）。