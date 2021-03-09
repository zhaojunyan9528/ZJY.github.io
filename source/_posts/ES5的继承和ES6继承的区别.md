---
title: ES5的继承和ES6继承的区别
tags:
  - 前端
categories:
  - - 笔记
    - es6
date: 2021-03-08 16:41:36
---

## ES5的继承和ES6继承的区别

### ES5继承：

基本思想：利用原型链让一个引用类型的原型去继承另一个引用类型的属性和方法（通过prototype和构造函数实现）
本质：将父类添加到子类的原型上去。

```js
function GrandParent(){
    this.name = 'GrandParent';
    this.a = 3;
}
Parent.prototype = new GrandParent();
function Parent(){
    this.name = 'Parent';
    this.b = 2;
}
Child.prototype = new Parent();
function Child(){
    this.name = 'Child';
    this.c = 1
}
let child = new Child();
console.log(child); //{name:'child',c:1}
console.log(child.a); // 3
console.log(child.b); //2
console.log(child.c); //1
```

### ES6继承：

基本思想：通过extends实现继承，子类可以继承父类的所以方法和属性。子类必须在constructor()方法中调用super()方法，因为新建的子类没有this对象，而是继承了父类的this对象。
本质：通过extends实现继承，子类继承了父类的所有方法和属性

```js
class Person{
    constructor(){
        this.a = 100;
        console.log('Person constructor')
    }
    eat(){
        console.log('eat')
    }
    static play(){
        console.log('Play')
    }
}
class Student extends Person{
    constructor(){
        super();
        this.b = 1;
    }
    study(){
        console.log('study')
    }
}
let stu = new Student();
console.log(stu.a, stu.b);
stu.eat();
stu.study();
Student.play();
```