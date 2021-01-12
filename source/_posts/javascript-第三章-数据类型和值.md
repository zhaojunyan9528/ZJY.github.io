---
title: javascript-第三章-数据类型和值
date: 2021-01-10 21:19:21
tags:

- 前端

categories:

- [笔记, Javascript]

---

## 第三章 数据类型和值

计算机程序是通过操作值来运行的。能够表示并操作的值的类型称为数据类型。

javascript允许使用3种基本数据类型：数字、字符串、布尔。此外还支持另外2种小数据类型：null（空）和undefined（未定义），只定义了一个值。

除了基本数据类型外，javascript还支持复合数据类型-对象，是值的集合。对象有两种，一种是已命名的值的无序集合，另一种是有编号的值的有序集合。后者称为数组。

javascript还定义了另一种特殊的类型-函数，是具有可执行代码的对象。js为函数定义了专用的语法。

除了数组和函数还定义了其他专用的对象，这些对象不是新的数据类型而是新的对象类。Date类定义的表示日期的对象。

RegExp类定义的表示正则表达式的对象。Error类定义的是js程序中发生的语法和运行时错误的对象。

### 1.数字

在js中，不区分整点型和浮点型数字，所有数字都是浮点型的，采用ieee 754标准的64位浮点格式表示

#### 1.1 整型直接量(十进制)

```javascript
0   3   1000
```

整数表示从2<sup>-53</sup> 到2 <sup>53</sup>,使用超过尾数范围的数字，就会失去精确性，
有些整数的运算，是对32位进行的，（2<sup>-31</sup> 到 2<sup>31-1</sup>)

#### 1.2八进制和十六进制直接量

除了10进制的整型直接量，js还支持8进制和16进制直接量。

16进制中数字可以用0-9表示，字母可以用a(A)-f(F)表示，代表0-15之间数字。16进制直接量由’0x’ 或‘0X’开头，后面跟16进制数字串。

0x123 基数是16 //1*16*16+2*16+3

尽管ECMAScript标准不支持8进制的直接量，但是javascript的某些实现却允许你使用8进制的直接量。8进制是以0开头，后面跟随0-7直接的数字，

0377 基数是8 // 3*8*8+7*8+7

由于某些js实现支持8进制有些不支持，所以尽量不要使用0开头的整型直接量。

#### 1.3浮点型直接量

由整数部分，小数点，小数部分组成，此外还可以用指数计数法表示浮点型直接量，即实数后面跟字母e或者E，后面跟正号和负号，再加一个整型指数。表示前面的实数乘以10的指数次幂。

```javascript
3.14
344.89
6.34e12 //6.34*1012
```

#### 1.4特殊数值

```javascript
Infinity  无穷大
-Infinity  负无穷大
NaN    非数字的特殊值，和任何值都不相等，包括自己，用专门的isNaN()函数来检测这个值。
isFinite()函数来测试一个值是否是NaN，正无穷大或负无穷大。
Number.MAX_VALUE  可表示的最大数字
Number.MIN_VALUE   可表示的最小数字（接近于0）
Number.NaN  非数字的特殊值
Number.POSITION_INFINITY 表示正无穷大的特殊值
Number.NEGATIVE_INFINITY 表示负无穷大的特殊值
```

### 2.字符串

由unicode字符、数字和标点符号组成的序列，表示文本的数据类型。

#### 2.1 字符串直接量

由单引号或者双引号扩起来的unicdoe字符序列。可以包含0个或多个字符序列

#### 2.2 字符串直接量中的转义字符

反斜杠\后加一个字母就可以表示特殊的用法。\n表示换行符
'you\'re right', \’表示单引号或者撇号

### 3.布尔值

true和false两个值，通常用于控制结果

### 4.函数

一段可执行的代码段，由js定义或由js预定义，可多次执行和调用。js函数是一个真正的数据类型，可以对函数进行操作。

Math.sin()预定义函数

#### 4.1 函数直接量

有关键字function和可选对参数名、用括号括起来对参数列表和花括号括起来定义的。

除了用函数定义来定义函数：

function square(x){ return x*x }

还可以用函数直接量类定义：

var square = funtion(x) { return x*x }

如果一个函数值存储在某个对象的属性中，那个这个函数通常被称为方法，属性名称被称为方法名。

### 5.对象

已命名的数据的集合，每个数值都有一个名字,称为对象的属性，通过"."操作符和属性名访问。

#### 5.1 创建对象

```javascript
var o = new Object();
var now = new Date();
var reg = new RegExp(/javascript/gi);
```

#### 5.2 对象直接量

var point = {x: 1,y:3}，
对象直接量可以嵌套，属性值可以不是常量。

### 6.数组

数值的集合，数组的每个数值都有一个下标，js不支持多维数组，但数组元素可以是数组。数组下标是非负整数

#### 6.1 数组的创建

可以通过构造函数Array()创建:

```javascript
var arr = new Array();
arr[0] = 1.2;
arr[1] = ‘javascript’;
var arr = new Array(1.2,’javascript’);
var arr = new Array(10); 创建的是表示10个未定义的数组

```

#### 6.2 数组直接量

var arr = [1,2,3,4];
数组可以嵌套，var arr = [[1,2,3],2];
数组元素不局限于常量：var arr = [1024,1024+1]
数组可以存放未定义的元素：var arr = [1,,,,5]

### 7.null

表示“无”值，尝被看作对象类型的特殊值，表示“无对象”的值。js保留字

### 8.undefined

当你使用一个未定义的值，或已定义未赋值的值，或不存在的对象属性时，返回这个值。

null == undefined  //true

如果要区分null和undefined可用 === 或者typeof区分

```javascript
typeof null // “object”  
typeof undefined // undefined
null === undefined // false
undefined 不是js保留字
ECMAScript v3 定义了名为undefined的全局变量，值为undefined
```

### 9.Date对象

表示日期和时间的对象类。可以用运算符new和构造函数Date()创建Date对象

### 10.正则表达式

用于模式匹配和查找替换操作，正则表达式直接量：/^html/

### 11.Error

表示错误的类,每个Error对象都有一个message属性，存放js特定错误消息。预定义的错误对象的类型有Error, EvalError, RangeError, ReferenceError, SyntaxError, TypeError和URIError.

### 12.基本数据类型的包装对象

var str =”lfjlajdfasd”;
var result = str.substring(1,4);
三个基本的数据类型都有一个相应的对象类，js不仅有数字、字符串、布尔类型，还有Number、String、Bool类，这些类是基本数据类型的包装，这些类不仅有基本数据类型的值，还定义了用来运算的属性和方法