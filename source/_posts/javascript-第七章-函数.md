---
title: javascript-第七章-函数
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-13 15:52:09
---

## 第七章-函数

### 1. 函数定义和调用

定义函数最常用方法是function语句，该语句由function关键字构成，后面跟：

+ 函数名
+ 一个括号括起来的参数列表，参数可选，多个参数用逗号分隔
+ 构成函数主体的javascript语句，大括号括起来

定义函数时可以使用个数可变的参数，而且函数既可以有return语句，也可以没有return语句，return语句使函数停止运行，并把结果返回给函数调用者。如果没有return语句，返回给调用者undefined.

### 1.1 嵌套的函数

```javascript
function test(){
    funtion add(a,b){
        return a+b
    }
    return add(1,2)
}
```

### 1.2 Function构造函数

function语句并不是创建函数的唯一方式，在ECMAScript v1和javascript 1.1中还可以使用Function()构造函数和new运算符。
var f = new Function(“x”,”y”,”return x*y;”)
Function构造函数可以接收任意数量参数，最后一个参数是函数主体，没有参数可只传函数主体字符串参数。

### 1.3 函数直接量

函数直接量是一个表达式，可以定义一个匿名函数。

```javascript
funtion f(x,y){	return x*y；} //function语句
var f = new Function(“x”,”y”,”return x*y;”)   //Function构造函数
var  f = function(x,y) { return x*y; } //函数直接量
```

虽然函数直接量定义的是未命名函数，但也可以指定名称，在递归函数非常有用

```javascript
var f = funtion fact(x){
    if(x<1){
        return 1;
    }else{
        return x*fact(x-1)；
    }
}
```

上面代码虽然定义了一个未命名函数，并把对他引用存储在变量f中，但并没有真正创建一个fact函数，只允许用这个名称引用自身

和Function构造函数一样，函数直接量创建的是匿名函数，而且不会自动地将这个函数存储在属性中。但是比起构造函数来，函数直接量又一个明显的优点。由Function构造函数创建的函数的主体，必须用一个字符串说明，用这种方式表达一个长而复杂的函数是非常笨拙的。
但是函数直接量的主体确是使用了javascript的语法。而且函数直接量只被解析和编译一次，而作为字符串传递给Function()函数的javascript代码则在每次调用该构造函数时只需被解析和编译一次。


### 2. 作为数据的函数

除了作为变量的值，还可以作为对象的属性，称为方法

### 3. 函数的作用域：调用对象

javascript函数的主体是在局部作用域中执行的，这个作用域通过把调用对象添加到作用域的头部创建的。
因为调用对象是作用域链的一部分，所以在函数体内可以把这个对象属性作为变量来访问。
var语句声明的变量作为调用对象的属性，函数形参也可用于对象的属性。

除了局部变量和形式参数外，调用对象还定义了一个属性：arguments，这个属性引用了另外一个Arguments对象。arguments应作为保留字。

### 4. 函数的实际参数：Arguments对象

在一个函数体内，标识符arguments具有特殊含义。他是调用对象的一个属性，用来引用Arguments对象。Arguments对象就像一个数组，可以通过数字获取传递给函数的参数值。
但它并非真正但Array对象。用参数名改变参数值也会改变通过arguments[]取的值。

#### 4.1 属性callee

除了数组元素，arguments对象还定义了callee属性。用来引用当前正在执行的函数。对未命名的函数非常有用。

```javascript
function(x){
    if(x<1) return 1;
    return x * arguments.callee(x-1);
}
```

### 5. 函数的属性和方法

在javascript程序中，函数可以用作数值，也可以通过Function()构造函数创建函数，所以函数是对象，Function对象，拥有属性和方法。

#### 5.1 属性length

在函数主体中，arguments数组的length属性指定了函数实际参数数目。
但是函数自身的length属性并非如此，它是只读特性，返回的是函数形式参数列表中声明的形式参数的数目。

```javascript
function check(args){
    var len = args.length; //实际的参数个数
    var actualLen = args.callee.length //声明的参数个数
}
```

#### 5.2 属性prototype

每个函数都有一个prototype属性，引用的是预定义的原型对象。原型对象在使用new运算符把函数作为构造函数时起作用。它在定义新的对象类型时起着非常重要的作用。

#### 5.3 定义你自己的函数属性

当函数需要在调用过程中始终不变的一个值时，使用Function对象的属性比定义一个全局变量更加方便。

```javascript
test.counter = 0;
function test(){
    return test.counter++;
}
test(); //0
test(); //1
```

#### 5.4 方法apply()和call()

所有函数都定义了apply()和call()方法。
第一个参数都是要调用的函数对象，在函数体内这个关键词是this的值。
call()的剩余参数是传递给调用函数的值，例如要把两个数字传递给函数f，并把它作为对象o的方法调用，可以使用如下代码：

```javascript
    f.call(o, 1, 2);
```

apply()要传递的参数是一个数组：

```javascript
    f.apply(o, [1, 2]);
```

比如要找到数字数组中最大值，可以用apply方法把数组传递给Math.max:

```javascript
    var biggest = Math.max.apply(null, array_number);
```



