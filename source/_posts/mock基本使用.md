---
title: mock基本使用
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-05-07 13:48:52
---

## Mock：生成随机数据，拦截 Ajax 请求

### 1.安装使用
Bower

```js
npm install -g bower
bower install --save mockjs
```

```js
<script type="text/javascript" src="./bower_components/mockjs/dist/mock.js"></script>
```
### 2.基础使用

```js
// String
var data = Mock.mock({
  "String|1-10": "*"
})
console.log(data)
// {String:'***'}

var data = Mock.mock({
  'string|3': 'a'
})
console.log(data)
// {string: "aaa"}


// Number
var data = Mock.mock({
  "number|1-100": 202
})
console.log(data)
// {number:57}

var data = Mock.mock({
  "number|1-100.1-10": 1
})
console.log(data)
// {number: 22.772934}

// Boolean
var data = Mock.mock({
  "boolean|1": true
})
console.log(data)
// {boolean:true}


// Object
var data = Mock.mock({
  "object|2": {
    "310000": "上海市",
    "320000": "江苏省",
    "330000": "浙江省",
    "340000": "安徽省"
  }
})
console.log(data)
// {
//   "object": {
//     "330000": "浙江省",
//     "340000": "安徽省"
//   }
// }

var data = Mock.mock({
  "object|2-4": {
    "110000": "北京市",
    "120000": "天津市",
    "130000": "河北省",
    "140000": "山西省"
  }
})
console.log(data)
// {
//   "object": {
//     "110000": "北京市",
//     "120000": "天津市",
//     "140000": "山西省"
//   }
// }


// Array
var data = Mock.mock({
  "array|2": [
    "AMD",
    "CMD",
    "UMD"
  ]
})
console.log(data)
// 随机产生一个元素
// {
//   "array": "AMD"
// }

var data = Mock.mock({
  "array|1-10": [
    "Mock.js"
  ]
})
console.log(data)
// 随机产生1-10个数组元素
// {
//   "array": [
//     "Mock.js",
//     "Mock.js"
//   ]
// }


// RegExp
var data = Mock.mock({
  'regexp': /[a-z][A-Z][0-9]/
})
console.log(data)
// {regexp: "jS2"}

var data = Mock.mock({
  'regexp': /\d{5,10}/
})
console.log(data)
// {regexp: "78998"}

var data = Mock.mock({
  'regexp|3': /\d{5,10}\-/
})
console.log(data)
// {
//   "regexp": "28739-1401541809-15613-"
// }


// 数据占位符定义

// 随机布尔值
Random.boolean()
Mock.mock('@boolean')
Mock.mock('@boolean()')

// 随机自然数
Random.natural()
Mock.mock('@natural')
Mock.mock('@natural()')

Random.integer()
Mock.mock('@integer')
Mock.mock('@integer()')

Random.float()
Mock.mock('@float')
Mock.mock('@float()')

Random.character()
Mock.mock('@character')
Mock.mock('@character()')

Random.string()
Mock.mock('@string')
Mock.mock('@string()')

Random.range(10) //[0,1,2,3,4,5,6,7,8,9]
Mock.mock('@range(10)') //[0,1,2,3,4,5,6,7,8,9]

// Date
Random.date()
Mock.mock('@date')
Mock.mock('@date()')

// Random.date( format )
Random.date('yyyy-MM-dd')
Random.date('yy-MM-dd')
Random.date('y-MM-dd')
Random.date('y-M-d')

Mock.mock('@date("yyyy-MM-dd")')
Mock.mock('@date("yy-MM-dd")')
Mock.mock('@date("y-MM-dd")')
Mock.mock('@date("y-M-d")')

Mock.mock('@date("yyyy yy y MM M dd d")')


Random.time()
Mock.mock('@time')
Mock.mock('@time()')

// Random.time( format )
Random.time('A HH:mm:ss')
Random.time('a HH:mm:ss')
Random.time('HH:mm:ss')
Random.time('H:m:s')

Mock.mock('@time("A HH:mm:ss")')
Mock.mock('@time("a HH:mm:ss")')
Mock.mock('@time("HH:mm:ss")')
Mock.mock('@time("H:m:s")')

Mock.mock('@datetime("HH H hh h mm m ss s SS S A a T")')

Random.datetime()
Mock.mock('@datetime')
Mock.mock('@datetime()')

Random.now()
Mock.mock('@now')
Mock.mock('@now()')

// 图片
// Random.image( size?, background?, foreground?, format?, text? )


Random.color()
Mock.mock('@color')
Mock.mock('@color()')

Random.rgb()
Random.hex()
Random.rgba()
Random.hsl()

Random.paragraph() // "Oksf fvugvxa oujindgp kmqyw iashota cdmukan tglfyxsfi kxbughdzn jvwt vepovtdog phnnjp nftibvwbw uqau. Xebwbu vpvrsff nxk cagcub fmglp svbpmajru dddi kvtuoggxm qpj ovheukv httjfkfir fpnhofukk. Lgtwosm riquhht ympctsdx yytczkwe tojuedety sdiysovrx wlutvml emmmjtmf yumoe jyiloiq cefuxpt mgefihrjbn wipsrenplw sskutcs mvfnjg mxdfv vsmcn cwneh. Mhcxlol hfhxcrvnm lzunjwn zeuvck nygd augt nfdafqsob awxrl bttybubefn rnricwwob xmqmng vxygxs qbfcv. Tnrugszs wjis qirkpf xiwrza hhmff wkmzpm xnswqsup jrhoyv ifc ufnsasp fagcgtcl vtmqoqunb ggeeftc qktbmf flritae wbq tudrsrb xoxqvqlb."



// Web
Random.url()
Mock.mock('@url')
Mock.mock('@url()')
// "wais://gelm.gf/tqkuz"
// "news://lkvi.so/nacax"
// "tn3270://zbxqguresa.fk/qguefcsa"

Random.domain()
Random.protocol()
Random.tld()
Random.email()
Random.ip()

// Address
Random.region() //"东北"
Random.province()
// Random.city( prefix? )
Random.city()
Mock.mock('@city')
Mock.mock('@city()')
// Random.city( prefix )
Random.city(true)
Mock.mock('@city(true)')
// "桃园县"
// "南昌市"
// "枣庄市"
// // Random.city( prefix )
// "广西壮族自治区 来宾市"
// "北京 北京市"
Random.county([prefix])

Random.zip() // "216873"

Random.guid() //"5D36E3Fb-dcF1-6CC3-d5A3-9feD7b2f4f52"
Random.id() // "500000200806155373"



let data = Mock.mock({
  'array|1-10':[{
      'name': Mock.mock('@name'),
      'age|18-69':1,
      'imageUrl': Mock.Random.image('200x100', '#894FC4', '#FFF', 'png', '!')
  }]
})
console.log(data)
// Array:6
// 0: {name: "Mark Hall", age: 28, imageUrl: "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"}
// 1: {name: "Mark Hall", age: 24, imageUrl: "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"}
// 2: {name: "Mark Hall", age: 37, imageUrl: "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"}
// 3: {name: "Mark Hall", age: 31, imageUrl: "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"}
// 4: {name: "Mark Hall", age: 48, imageUrl: "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"}
// 5: {name: "Mark Hall", age: 34, imageUrl: "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"}

```