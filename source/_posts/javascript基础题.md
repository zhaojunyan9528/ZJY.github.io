---
title: javascript基础题
tags:
  - 前端
categories:
  - - 面试
    - Javascript
date: 2021-01-20 11:56:25
---

### 1.js基础

+ 基本类型：
    + number
    + string
    + boolean
    + null
    + undefined
    + symbol  

基本类型的比较就是值的比较，访问的是值的本身，没有属性和方法，保存在栈内存中
        
+ 引用类型
    + Array
    + Function
    + Date
    + Object
        
引用类型有属性和方法，同时保存在栈内存和堆内存中；引用类型的比较是内存地址的比较。

```javascript
        let address = {details:'a'}
        let one = address
        let tow = address
        one === tow //true
        // 虽然one、tow是两枚不同的指针，但它们都同时指向了堆内存里的address的内容，所以它们是相等的
        
        // 函数参数是引用类型（对象）的时候会发生什么
        
        function test(person) {
          person.age = 26
          person = {
            name: 'aaa',
            age: 30
          }
        
          return person
        }
        const p1 = {
          name: 'bbb',
          age: 25
        }
        const p2 = test(p1)
        p1 // {age:26,name:'bbb'}
        p2 // {age:30,name:'aaa'}
```

函数传递参数时，实际上是传递指针的副本。
test(p1)实际上传递的就是p1的副本，这时指针仍然指向{name: 'bbb',age: 25}
person.age = 26 这时修改的还是原来内存位置的内容，所以这时p1的age变成了26
当person = {} .. 相当于重新开辟了一块堆内存，赋值{name:'aaa',age:30},最后返回这个对象，而这个对象的指针就是p2
        
### 2.字符串翻转：

console.log(str1.split('').reverse().join(''))
     
判断字符串出现次数最多元素，并统计次数

```javascript
    let str2 = 'asdfasdfasdfasdfjkjkljlkjssss'
    let obj = {}
    let num2 = str2.length
    for (let i = 0; i < num2; i++) {
      if (obj[str2.charAt(i)]) {
        obj[str2.charAt(i)] = parseInt(obj[str2.charAt(i)]) + 1
      } else {
        obj[str2.charAt(i)] = 1
      }
    }
    console.log(obj)
    let maxNum = 0
    let maxDocument = null
    for (let j in obj) {//循环对象
      if (obj[j] > maxNum) {
        maxNum = obj[j]
        maxDocument = j
      }
    }
```

### 3.数组去重

```javascript
    let arr = ['a','b','c','d','c','a','e'];
    let arr2 = [];
    for(let i=0;i<arr.length;i++){
      if(arr2.indexOf(arr[i])== -1){
        arr2.push(arr[i]);
      }
    }
    console.log(arr2)
```

### 4.replace字符串替换

```javascript
    let str = 'hello china';
    let str2 = str.replace('hello', 'hi');
    console.log(str2,str);//hi china,hello china
```

### 5.toUpperCase()大写/toLocaleUpperCase

### 6.toLowerCase()小写/toLocalLowerCase

### 7.repeat(count:number)字符串重复次数

### 8.Math.ceil()向上舍入 4.1结果5

### 9.Math.floor()向下舍入  4.9结果4

### 10.Math.round()把数四舍五入为最接近的整数。

### 11.数组[]和length=0的区别：

```javascript
    var foo = [1,2,3];
    var bar = [1,2,3];
    var foo2 = foo;
    var bar2 = bar;
    foo=[];
    bar.length = 0;
    console.log(foo,foo2,bar,bar2);//[]  [1,2,3]  []  []
```

[]是创建了一个新数组，重新分配了内存空间，任何其他引
用不受影响，仍指向其原始数据
length=0 修改数组本身。如果通过不同的变量访问它，那
么仍然可以获得修改后的数组

### 12.typeof

```javascript
    typeof null // "object"
    typeof undefined //"undefined"
    typeof NaN // "number"
    typeof Infinity // "number"
    typeof function(){}  //"function"
    
    typeof new String("aaa") //"object"
    typeof String('aaa') //"string"
    
    null instanceof object //false
    null instanceof 任何类型 //false
    
    typeof可以用来检查一个没有声明的变量，而不报错:
    typeof v //"undefined"
```

typeof可以判断基本数据类型：String，Boolean,Number,但不能判断Array,Object,Null类型

### 13.变量提升

```javascript
    var name = 'World!';
    (function () {
        if (typeof name === 'undefined') {
            var name = 'Jack';
            console.log('Goodbye ' + name);
        } else {
            console.log('Hello ' + name);
        }
    })();
    // 结果：Goodbye Jack
    // 注意js的var hoisting变量声明提升，虽然声明提升，但是初始化并不提升
    // 这段代码相当于：
    var name = 'World!';
    (function () {
        var name;
        if (typeof name === 'undefined') {
            name = 'Jack';
            console.log('Goodbye ' + name);
        } else {
            console.log('Hello ' + name);
        }
    })();
 ```

### 14.splice
    
splice(index,number,value)向数组中添加或删除数目，返回被删除的数目
    index：必须，要添加或删除的起始位置，为负时，从数组尾部开始
    number：必须，要删除的数量，为0时则不删除
    value：可选，要添加的数目
        
删除：

```javascript
    var a = [1,2,3];
    var b = a;
    console.log(a.splice(0,a.length)); //[1,2,3]
    console.log(a,b); //[] []
```

添加：

```javascript
   
    var arr = [1,2,3];
    var arr1 = arr;
    console.log(arr.splice(1,0,'a','b')); //[]
    console.log(arr,arr1);// [1,'a','b',2,3]   [1,'a','b',2,3]
``` 

### 15.slice

slice(start,end)向数组中选出指定数组，返回被截选的子数组
（start到end，不包含end），并不改变原数组
+ start：必须，规定从何处开始，为负时，从数组尾部算起,-1为尾部第一个元素
+ end：可选，规定截取到何处，没有此参数则到数组结尾所有元素，
    为负时，从尾部开始算起元素

```javascript
    var c = [1,2,3];
    var d = c;
    console.log(c.slice(0,2));//[1,2]
    console.log(c,d);//[1,2,3]  [1,2,3]
    
    // 还可以截取字符串：不包含end元素
    var test = 'hello,world';
    console.log(test.slice(1,5),test);// ello   hello,world
    console.log(test.slice(-3),test);// rld  hello,world
    
    // substring:不包含end元素
    var test = 'hello,world';
    console.log(test.substring(1,5),test);// ello   hello,world
    
    // substr:包含end元素
    var test = 'hello,world';
    console.log(test.substr(1,5),test);// ello,   hello,world
```

### 16.js判断数据类型

1.typeof:

```javascript
        typeof 'ls' // string
        typeof 123 // number
        typeof true // bollean
        typeof null // object
        typeof undefined // undefined
        typeof [] // object
        typeof {} // object
        typeof function(){} // function
        typeof Symbol(1) // symbol
```

typeof可以判断基本数据类型：String，Boolean,Number,但不能判断Array,Object,Null类型


2.instanceof

判断构造函数的prototype属性是否存在某实例对象的原型链上。

```javascript
        // instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。其意思就是判断对象是否是某一数据类型（如Array）的实例
        function A(){}
        let a = new A();
        console.log(a instanceof A);//true,因为 Object.getPrototypeOf(a) === A.prototype;
        
        console.log('srr' intanceof String) //false
        console.log(1 instanceof Number) // false
        console.log(true instanceof Boolean) // false
        在这里字面量值，2， true ，'str'不是实例，所以判断值为false
        console.log(new Number(2) instanceof Number)// true
        console.log(new String('2') instanceof String)// true
        console.log(new Boolean(false) instanceof Boolean)// true
        console.log([] instanceof Array) // true
        console.log({} instanceof Object) // true
        console.log(function(){} instanceof Function)// true
        
        // null和undefined
        conosle.log(null instanceof Null) // Null is not defined
        console.log(undefined instanceof Undefined) //Undefined is not undefined
        
        console.log(new null instanceof Null) // null is not a constructor
        console.log(new undefined instanceof Undefined) //undefined is not a constructor
        // 浏览器认为null，undefined不是构造器
``` 

能判断Array,Object,Null类型，但不能区分基本数据类型

3.constructor

constructor属性返回创建该对象时构造函数的引用

```javascript
        console.log((2).constructor === Number); //true
        console.log((true).constructor === Boolean);//true
        console.log(('str').constructor === String);//true
        console.log(([]).constructor === Array);//true
        console.log(({}).constructor === Object);//true
        console.log((function() {}).constructor === Function);//true
        
        // 用costructor来判断类型看起来是完美的，然而，如果
        // 我创建一个对象，更改它的原型，这种方式也变得不可
        // 靠了
        function Fn(){}
        Fn.prototype = new Array();
        var f = new Fn();
        console.log(f.constructor === Fn);//false
        console.log(f.constructor=== Array);//true
``` 

4.Object.prototype.toString.call

toString方法返回调用它的对象的类型或值。
返回值默认是[object class],class是对象的内部类型，通常对应对象的构造函数名。

```javascript  
        // 常用于判断浏览器内置对象,对于所有基本的数据类型都能进行判断，即使是 null 和 undefined
        var  test = Object.prototype.toString;
        console.log(test.call(1)); //[object Number]
        console.log(test.call(true)); //[object Boolean]
        console.log(test.call('123')); //[object String]
        console.log(test.call([])); //[object Array]
        console.log(test.call({})); //[object Object]
        console.log(test.call(function(){})); //[object Function]
        console.log(test.call(null)); //[object Null]
        console.log(test.call(undefined)); //[object Undefined]       
```

5.Array.isArray

```javascript   
   console.log(Array.isArray([]))// true
        
    var str = new String('123');
    console.log(typeof str); //object
    console.log(str instanceof String); //true
    console.log(str.constructor===String); //true
    console.log(Object.prototype.toString.call(str));[object String]
```

### 17.闭包的概念

闭包就是能访问其他函数内部的变量的函数
+ 优点：
    避免全局变量的污染
    变量长期存储在内存中（缓存变量）
+ 缺点：
    常驻内存，加大内存使用
    内存泄漏
闭包实现了私有变量和参数
    
### 18.深拷贝和浅拷贝

+ 基础数据类型（number string boolean null undefined）存储在栈内存中
+ 引用数据类型（Function Array Object）变量名与内存地址存储在栈内存中，值作为对象保存在堆内存中
+ 基础数据类型的比较是值比较
+ 引用数据类型的比较是内存地址比较
    
浅拷贝：只复制指向某个对象的指针，而不复制对象本身，新旧对象共享同一块内存
深拷贝：复制并创建一个一摸一样的对象，不共享内存，修改新对象，旧对象保持不变
    
+ 浅拷贝：
        Object.assign()
        扩展运算符...
        Array.prototype.slice()//数组浅拷贝
+ 深拷贝：

```javascript
        JSON.parse(JSON.stringify())
        // 递归克隆
        
        // 实现深拷贝：浅拷贝+递归
        // 浅拷贝：
        function cloneShallow(source) {
            var target = {};
            for (var key in source) {
                if (Object.prototype.hasOwnProperty.call(source, key)) {
                    target[key] = source[key];
                }
            }
            return target;
        }

        // 测试用例
        var a = {
            name: "muyiy",
            book: {
                title: "You Don't Know JS",
                price: "45"
            },
            a1: undefined,
            a2: null,
            a3: 123
        }
        var b = cloneShallow(a);
        
        a.name = "高级前端进阶";
        a.book.price = "55";
        
        console.log(b);
        // { 
        //   name: 'muyiy', 
        //   book: { title: 'You Don\'t Know JS', price: '55' },
        //   a1: undefined,
        //   a2: null,
        //   a3: 123
        // }
        // 以上只能拷贝一层，只要稍微改动下，加上是否是对象
        // 的判断并在相应的位置使用递归就可以实现简单深拷贝。
        
        function cloneDeep1(source) {
            var target = {};
            for(var key in source) {
                if (Object.prototype.hasOwnProperty.call(source, key)) {
                    if (typeof source[key] === 'object') {
                        target[key] = cloneDeep1(source[key]); // 注意这里
                    } else {
                        target[key] = source[key];
                    }
                }
            }
            return target;
        }
        
        // 使用上面测试用例测试一下
        var b = cloneDeep1(a);
        console.log(b);
        // { 
        //   name: 'muyiy', 
        //   book: { title: 'You Don\'t Know JS', price: '45' }, 
        //   a1: undefined,
        //   a2: {},
        //   a3: 123
        // }
```

一个简单的深拷贝就完成了，但是这个实现还存在很多问题。

1、没有对传入参数进行校验，传入 null 时应该返回 null 而不是 {}

2、对于对象的判断逻辑不严谨，因为 typeof null === 'object'

3、没有考虑数组的兼容
        
判断是否是object

```javascript
        function isObject(obj) {
        	return typeof obj === 'object' && obj != null;
        }
        // 所以兼容数组的写法如下：
        functoin cloneDeep2(source){
            if(!isObject(source)) return source;//非对象返回自身
            
            var target = Array.isArray(source) ? []: {};
            for(var key in source){
                if(Object.prototype.hasOwnProperty.call(source,key)){
                    if(isObject(source[key])){
                        target[key] = cloneDeep2(source[key])
                    }else{
                        target[key] = source[key]
                    }
                }
            }
            return target;
        }
```

赋值、浅拷贝、深拷贝区别：

+ 赋值：赋值就是将某一数值或某一对象赋给某个变量的过程，分为如下两个部分：
        基本数据类型：赋值，赋值之后两个变量互不影响
        引用数据类型：赋址，两个变量具有相同的引用，指向同一个对象，互相有影响
        
对基本类型进行赋值操作，两个变量互不影响：

```javascript
        let a = "muyiy";
        let b = a;
        console.log(b);
        // muyiy
        
        a = "change";
        console.log(a);
        // change
        console.log(b);
        // muyiy
```

对引用类型进行赋址操作，两个变量指向同一个对象，改变变量 a 之后会影响变量 b，哪怕改变的只是对象 a中的基本类型数据。

```javascript
        let a = {
            name: "muyiy",
            book: {
                title: "You Don't Know JS",
                price: "45"
            }
        }
        let b = a;
        console.log(b);
        // {
        // 	name: "muyiy",
        // 	book: {title: "You Don't Know JS", price: "45"}
        // } 
        
        a.name = "change";
        a.book.price = "55";
        console.log(a);
        // {
        // 	name: "change",
        // 	book: {title: "You Don't Know JS", price: "55"}
        // } 
        
        console.log(b);
        // {
        // 	name: "change",
        // 	book: {title: "You Don't Know JS", price: "55"}
        // } 
```

+ 浅拷贝（Shallow Copy）：
创建一个对象，这个对象有原始对象属性值的一份精确拷贝。如果属性值是基本数据类型，拷贝的就是基本数据类型的值，如果属性值是引用类型，拷贝的就是内存的地址，所以两个对象会相互影响。浅拷贝只解决了第一层的问题，拷贝第一层的基本类型值，以及第一层的引用类型地址
        
    浅拷贝使用场景：

```javascript   
            1.Object.assign()方法将所有可枚举属性的值从一个或多个源对象复制到目标对象
            // 木易杨
            let a = {
                name: "muyiy",
                book: {
                    title: "You Don't Know JS",
                    price: "45"
                }
            }
            let b = Object.assign({}, a);
            console.log(b);
            // {
            // 	name: "muyiy",
            // 	book: {title: "You Don't Know JS", price: "45"}
            // } 
            
            a.name = "change";
            a.book.price = "55";
            console.log(a);
            // {
            // 	name: "change",
            // 	book: {title: "You Don't Know JS", price: "55"}
            // } 
            
            console.log(b);
            // {
            // 	name: "muyiy",
            // 	book: {title: "You Don't Know JS", price: "55"}
            // } 
```

上面代码改变对象 a 之后，对象 b 的基本属性保持不变。但是当改变对象 a 中的对象 book 时，对象 b 相应的位置也发生了变化

2.展开语法 Spread

```javascript
            let a = {
                name: "muyiy",
                book: {
                    title: "You Don't Know JS",
                    price: "45"
                }
            }
            let b = {...a};
            console.log(b);
            // {
            // 	name: "muyiy",
            // 	book: {title: "You Don't Know JS", price: "45"}
            // } 
            
            a.name = "change";
            a.book.price = "55";
            console.log(a);
            // {
            // 	name: "change",
            // 	book: {title: "You Don't Know JS", price: "55"}
            // } 
            
            console.log(b);
            // {
            // 	name: "muyiy",
            // 	book: {title: "You Don't Know JS", price: "55"}
            // } 
```

通过代码可以看出实际效果和 Object.assign() 是一样的。
            
3.Array.prototype.slice()

slice() 方法返回一个新的数组对象，这一对象是一个由 begin和 end（不包括end）决定的原数组的浅拷贝。原始数组不会被改变。

```javascript
            let a = [0, "1", [2, 3]];
            let b = a.slice(1);
            console.log(b);
            // ["1", [2, 3]]
            
            a[1] = "99";
            a[2][0] = 4;
            console.log(a);
            // [0, "99", [4, 3]]
            
            console.log(b);
            //  ["1", [4, 3]]
```

深拷贝（Deep Copy）
        
复制并创建一个一摸一样的对象，不共享内存，修改新对象，旧对象保持不变;
        
深拷贝使用场景：

```javascript
            1。JSON.parse(JSON.stringify(object))

            let a = {
                name: "muyiy",
                book: {
                    title: "You Don't Know JS",
                    price: "45"
                }
            }
            let b = JSON.parse(JSON.stringify(a));
            console.log(b);
            // {
            // 	name: "muyiy",
            // 	book: {title: "You Don't Know JS", price: "45"}
            // } 
            
            a.name = "change";
            a.book.price = "55";
            console.log(a);
            // {
            // 	name: "change",
            // 	book: {title: "You Don't Know JS", price: "55"}
            // } 
            
            console.log(b);
            // {
            // 	name: "muyiy",
            // 	book: {title: "You Don't Know JS", price: "45"}
            // } 
```

对数组深拷贝效果如何：

```javascript
            let a = [0, "1", [2, 3]];
            let b = JSON.parse(JSON.stringify( a.slice(1) ));
            console.log(b);
            // ["1", [2, 3]]
            
            a[1] = "99";
            a[2][0] = 4;
            console.log(a);
            // [0, "99", [4, 3]]
            
            console.log(b);
            //  ["1", [2, 3]]
```

对数组深拷贝之后，改变原数组不会影响到拷贝之后的数组但是该方法有以下几个问题：
会忽略 undefined/symbol，不能处理正则/new Date
undefined、symbol 和函数这三种情况，会直接忽略

```javascript
            let obj = {
                name: 'muyiy',
                a: undefined,
                b: Symbol('muyiy'),
                c: function() {}
            }
            console.log(obj);
            // {
            // 	name: "muyiy", 
            // 	a: undefined, 
            //  b: Symbol(muyiy), 
            //  c: ƒ ()
            // }
            
            let b = JSON.parse(JSON.stringify(obj));
            console.log(b);
            // {name: "muyiy"}
```

### 19.数组去重：
    
1.for循环嵌套for循环，使用splice去重更改原数组 正向遍历循环

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct (arr) {
            for(let i = 0; i < arr.length; i++) {
                for(let j = i + 1; j < arr.length ; j++) {
                    if(arr[i] === arr[j]) {
                        arr.splice(j, 1)
                        j--;
                    }
                }
            }
        } 
        
        distinct(arr)
        console.log(arr) // [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]
    }
```

优点：该方法可以顾虑到重复的 String、Boolean、 Number、undefined、null，返回的是去重后的原数组。
缺点：不能过滤掉 NaN、Object
    
2.for循环嵌套for循环，使用splice去重更改原数组 逆向遍历循环

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct (arr) {
            for(let i = arr.length; i > 0; i--) {
                for(let j = i - 1; j > -1 ; j--) {
                    if(arr[i] === arr[j]) {
                        console.log(arr[j])
                        arr.splice(j, 1)
                    }
                }
            }
        } 
        
        distinct(arr)
        console.log(arr) // [1, "true", true, 15, false, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]
    }
```

优缺点同方法一
    
3.includes去重 返回新数组

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct (arr) {
            let newArr = []
            for(let i = 0; i < arr.length; i++) {
                if(!newArr.includes(arr[i])) {
                    newArr.push(arr[i])
                } 
            }
            return newArr
        }
        
        console.log(distinct(arr)) // [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
    }
```

优点是可以过滤重复的NaN了，但是返回的是个新数组，对比方法一二，该方法多消耗了一些存储空间。

优点：该方法可以顾虑到重复的 String、Boolean、Number、undefined、null、NaN，返回的是去重后的新数组。
缺点：不能过滤掉 Object
    
4.indexOf去重 返回新数组

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct (arr) {
            let newArr = []
            for(let i = 0; i < arr.length; i++) {
                if(newArr.indexOf(arr[i]) < 0) {
                    newArr.push(arr[i])
                } 
            }
            return newArr
        }
        
        console.log(distinct(arr)) //  [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]
    }
```

优点：该方法可以顾虑到重复的 String、Boolean、 Number、undefined、null，返回的是去重后的新数组。
缺点：不能过滤掉 NaN、Object

5.利用对象的属性key唯一的特性去重

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct(arr) {
            let obj = {}
            let newArr = []
            for(let i = 0; i < arr.length; i++) {
                if(!obj[arr[i]]){
                    obj[arr[i]] = 1
                    newArr.push(arr[i])
                }
            }
            return newArr
        }
    
        console.log(distinct(arr)) // [1, "true", 15, false, undefined, null, NaN, 0, "a", {…}]
    }
```

该方法不仅可以过滤掉重复的NaN,还是可以过滤掉Object。

优点：该方法可以顾虑到重复的 String、Boolean、 Number、undefined、null、NaN、Object，返回的是去重后的新数组。
缺点：针对 NaN和'NaN', 对象的key会视为一个key，区分不了NaN和'NaN'
    
6.利用ES6的Set数据结构的特性

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct(arr) {
            return Array.from(new Set(arr))
        }
    
        console.log(distinct(arr)) // [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
    }
```

优点：该方法可以顾虑到重复的 String、Boolean、 Number、undefined、null、NaN，返回的是去重后的新数组。
缺点：不能过滤重复的Object
    
7.利用ES6的Map数据结构的特性去重

```javascript
    {
        let arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN','NaN', 0, 0, 'a', 'a',{},{}];
        function distinct(arr) {
            let map = new Map()
            let newArr = []
            for(let i = 0; i < arr.length; i++) {
                if(!map.has(arr[i])) {
                    map.set(arr[i])
                    newArr.push(arr[i])
                }
            }
            return newArr
        }
    
        console.log(distinct(arr)) // [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
    }
```

优点：该方法可以顾虑到重复的 String、Boolean、 Number、undefined、null、NaN，返回的是去重后的新数组。
缺点：不能过滤重复的Object


总结：
+ splice、indexof（不能过滤NaN、object）：
        双重for循环，判断arr[i] 和 arr[j](j=i+1)是否相等利
        用splice去重数组，正向遍历数组，不能过滤掉 NaN、Object
        双重for循环，判断arr[i]和arr[j](j=i-1)是否相等利
        用splice去重数组，逆向遍历数组，不能过滤掉 NaN、Object
        定义新数组，for循环判断新数组是否包含indexOf当前循环值，
        利用indexOf去重，不能过滤掉 NaN、Object
    
+ includes、set、map（不能过滤object）：
        定义新数组，for循环判断新数组是否includes当
        前循环值，利用includes去重，不能过滤掉 Object
        利用ES6的Set数据结构的特性：return Array.from
        (new Set(arr))，不能过滤掉 Object
        利用ES6的Map数据结构的特性去重：
            let map = new Map()
            for循环中判断：map.has(arr[i])
        不能过滤掉 Object
        
+ 利用对象的属性key唯一的特性去重:
        let obj = {}
        for循环判断：obj[arr[i]]不存在将当前循环元素push新数组
        可以过滤掉 NaN、Object，不可以区分'NaN'和NaN
        
        
### 20.DOM 事件有哪些阶段？谈谈对事件代理的理解

捕获阶段-目标阶段-冒泡阶段

事件代理简单说就是：事件不直接绑定到某元素上，而是绑定到该元素的父元素上，进行触发事件操作时(例如'click')，再通过条件判断，执行事件触发后的语句(例如'alert(e.target.innerHTML)')

好处：(1)使代码更简洁；(2)节省内存开销
    
### 21.async/await

async用于声明一个函数是异步的，await用来等待一个异步
方法执行完成，await只能出现在async函数中

async函数会返回一个promise对象，如果async函数直接
return一个直接量，async函数会把这个直接量用promise.resolve()
封装成promise对象。

```javascript   
        async function testAsync() {
            return "hello async";
        }
        
        const result = testAsync();
        console.log(result);//Promise {resolved： 'hello async' }
```

所以如果不能用await取其返回值情况下可用：
then()链来处理这个promise对象：

```javascript
        testAsync().then(res => {
            console.log(res); //hello async
        })
```

如果async没有返回值，它会返回Promise.resolve(undefined)

await等待的是一个表达式，这个表达式的结果是promise对
象或其他值（直接量或者普通函数）

```javascript       
        function getSomething() {
            return "something";
        }
        
        async function testAsync() {
            return Promise.resolve("hello async");
        }
        
        async function test() {
            const v1 = await getSomething();
            const v2 = await testAsync();
            console.log(v1, v2);
        }
        
        test();//something hello async
```

如果await等待的不是promise对象，那await表达式的结果就是它等到的东西
如果await等待的是promise对象，那它就阻塞后面的代码，等着
promise对象resolve，resolve的值就是await表达式的运算结果

async会将其后的函数的返回值封装成一个promise对象，而await会等待这个promise完成，并将resolve结果返回

await函数结果可能是rejected，所以最好把await命令放
在try...catch块中

```javascript
        async function test(){
            try{
                await getSomething();
            }catch(err){
                console.log(err)
            }
        }
        // 另一种写法
        async function test(){
            await getSomething().catch(err=>{
                    console.log(err)
                }
            )
        }
```

### 22.协程

意思是多个线程互相协作，完成异步任务。
协程有点像函数，又有点像线程。它的运行流程大致如下：
第一步，协程A开始执行。

第二步，协程A执行到一半，进入暂停，执行权转移到协程B。

第三步，（一段时间后）协程B交还执行权。

第四步，协程A恢复执行。
        
### 23.Generator

Generator函数就是协程再es6的实现，最大的特点就是可用交出函数的执行权（暂停执行）

```javascript
function* gen(x){
  var y = yield x + 2;
  return y;
}
```
    
上面代码就是一个 Generator 函数。它不同于普通函数，是可以暂停执行的，所以函数名之前要加星号，以示区别。整个generator函数就是一个异步任务，需要操作暂停地方就用yeild语句注明

```javascript
var g = gen(1);
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
```

generator函数返回一个内部指针g，执行generator函数不会返回结果，而是返回一个指针对象，调用g的next方法会移动内部指针移动到yeild语句

next 方法的作用是分阶段执行 Generator 函数，每次调用next方法会返回一个对象，表示当前阶段信息（value和done）value是yeild语句后表达式的值，done 属性是一个布尔值，表示Generator 函数是否执行完毕，即是否还有下一个阶段。

Generator函数可以暂停执行和恢复执行，这是可用异步任务的原因，除此外还有两个特效：函数体内数据交换和错误处理机制

```javascript
function* gen(x){
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }
```

第二个 next 方法带有参数2，这个参数可以传入 Generator函数，作为上个阶段异步任务的返回结果，被函数体内的变量 y 接收。因此，这一步的 value 属性，返回的就是2
