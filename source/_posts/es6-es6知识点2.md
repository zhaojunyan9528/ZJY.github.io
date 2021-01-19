---
title: es6-es6知识点2
tags:
  - 前端
categories:
  - - 笔记
    - es6
date: 2021-01-19 17:49:17
---

1.变量：--2019/9/26

    var:
    1.可以重复声明
    2.没有块级作用域
    3.不能限制

    let/const:
    1.禁止重复声明
    2.限制修改（const)
    3.支持块级作用域
        if(true){
            var a= 5;
        }
        alert(a) //正常执行
        if(true){
            let a= 5;
        }
        alert(a) //报错：Uncaught ReferenceError: a is not defined

2.结构赋值：

    1.左右两边一样：
        let {a,b} = {a:1,b:2}
        let [a,b] = [a:1,b:2]
    2.右边得是个东西：
        let {a,b} = {1,2} // error

3.箭头函数：

    () => {}
    1.有且仅有一个参数： ()可以省略： param => {}
    2.有且仅有一条语句：{}可以省略： i => return i++

    修复this

4.收集剩余参数：剩余参数必须是最后一个参数

    let json = {a:1,b:2,c:4}
    let {a,...b} = json //b:{b:2,c:4}

5.展开参数：

    let arr1 = [1,2,3]
    let arr2 = [1,2,3]
    let arr3 = [...arr1,...arr2]

6.系统对象

    Array
        1.map  映射
        2.reduce 求和 求平均
        3.forEach 遍历
        4.filter 过滤
    String
        字符串模板： `${}`
        startWith
        endsWith
    JSON
        标准写法：
            {"key":value}
        JSON对象：
            JSON.Stringify
            JSON.parse

7.异步处理

promise
async/await

1.promise用法

```javascript
    let p = new Promise(function(resolve,reject){
            $.ajax({
                url:'./test.json',
                dataType: 'json',
                success(data){
                    resolve(data)
                },
                error(res){
                    reject(res)
                }
            })
        })
        p.then(res=>{
            console.log('success',res);
        },err=>{
            console.log('fail',err);
        })
```

2.promise.all() 全部执行完再返回结果，一个失败全都失败

```javascript
    Promise.all([
            $.ajax({url:'./test.json',dataType: 'json'}),
            $.ajax({url:'./test.json',dataType: 'json'}),
            $.ajax({url:'./test.json',dataType: 'json'})
        ]).then(arr=>{
            console.log(arr);
        },err=>{
            console.log(err);
        })
```

3.async/await 用同步写法写异步操作

```javascript
    async function test(){
            let data1 = await $.ajax({url:'./test.json',dataType: 'json'});
            let data2 = await $.ajax({url:'./test.json',dataType: 'json'});
            let data3 = await $.ajax({url:'./test.json',dataType: 'json'});
            console.log(data1,data2,data3);
        }
```

4.babel: 

    1.在线：
    type="text/babel"
    2.编译
    安装node
    npm install --save-dev @babel/core @babel/cli @babel/preset-env
    添加脚本： npm init -y 生成package.json文件 script添加脚本
    单文件编译：添加.babelrc文件---声明preset
    希望编译 node_modules 目录下的模块：创建babel.config.js

5.ES6(ES2015)--2019/9/27

    ES7: .. Array.includes()
    ES8:  async/await
    ES9: rest/spread  异步迭代 Promise.finally()  正则

6.面向对象
机器语言-> 汇编 -> 低级语言（面向过程）->高级语言（面向对象）-> 模块 -> 框架 ->API
面向对象3大特性：
1.封装性
2.继承性
3.多态性

es5实现面向对象：
1.类

```javascript
function Person(name,age){
    this.name = name
    this.age = age
}
Person.prototype.show = function() {
    console.log(this.name,this.age);
}
var p = new Person('test',14)
p.show()
```

2.继承：
```javascript
        function Worker(name,age,job){
            Person.call(this, name, age)
            this.job = job
        }

        Worker.prototype = new Person()
        Worker.prototype.constructor = Worker

        Worker.prototype.workShow = function() {
            console.log(this.job);
        }

        let work = new Worker('es5',14,'test')
        work.show()
        work.workShow()
```

ES6实现面向对象：class关键字  constructor构造器
1.类

```javascript
        class Person{
            constructor(name,age){
                this.name = name
                this.age = age
            }
            show(){
                console.log(this.name,this.age);
            }
        }
        let p = new Person('blue',15);
        p.show()
```

2.继承 extends super

```javascript
        class Worker extends Person{
            constructor(name,age,job){
                super(name,age)
                this.job = job
            }
            workShow(){
                console.log(this.job);
            }
        }

        let work = new Worker('es56',17,'rtesf')
        work.show()
        work.workShow()
```

7.闭包 

函数执行完毕内存不回收

```javascript
    for(var i=0; i< button.length; i++){
        (function(i){
            button[i].onclick = function() {
                console.log(i);
            }
        })(i)
    }
```

8.ES6模块化

