---
title: javascript-第九章-数组
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-13 17:47:04
---

## 第九章-数组

对象类型是一种包含已命名的的值的复合数据类型，数组即一种包含已编码的值的复合数据类型

### 1. 数组和数组元素

数组（array）是一种数据类型，它包含或者存储了编码的值。每个编码的值称作该数组的一个元素（element），每个元素的编码成为数组的下标（index）。

#### 1.1 数组的创建

```javascript
// 1
var a = new Array(1,2,3,'a')
a
[1,2,3,'a']
// 2
var a = new Array(3);
a.length; //3, 每个元素都是undefined
// 3
var a = new Array();//[] 创建了一个空数组
// 
var a = ['a','b',1,2]
```

#### 1.2 数组元素的读和写

可以用[]来存取数组元素。方括号左边是对数组的引用，方括号中是具有非负整数的任意表达式。数组的下标必须是大于等于0并小于2<sup>23-1</sup>的整数，如果你使用的数字过大或者使用了小数负数，浮点数，会转化为字符串作为对象属性的名字，例如：

```javascript
var a = [1,2]
a[-12] = 12
a
[1, 2, {-12:12}]
```

#### 1.3 数组的长度

所有的数组都有length属性，用来说明这个数组包含的元素个数。数组的length是自动更新的。

```javascript
var a = new Array();
a.length
0
a = new Array(10);
a.length
10
```

数组的length属性既可以读又可以写，当length设置了一个比他小的值，之后的值会被截断，值就会丢失。如果length设置的值比当前值大，那么未定义的元素就会被添加到数组末尾以使数组增加到指定长度。
通过设置数组length是唯一缩短数组长度的方法。如果使用delete来删除元素，虽然元素变成未定义的，但是数组的length属性不会变。

### 2. 数组的方法

#### 2.1 join方法

方法Array.join()方法可以将一个数组元素都转换成字符串，然后拼接起来。你可以指定一个可选的字符串来隔离结果字符串中的元素，如果没有指定，用逗号分割。

```javascript
var a = [1,2];
a.join("*")
"1*2"
```

#### 2.2.reverse方法

方法Array.reverse()方法将颠倒数组顺序并返回颠倒后的结果。他在原数组上操作，并不是新建一个数组进行操作：

```javascript
var a = [1,2,3]
a.reverse();
[3, 2, 1]
a
[3, 2, 1]
```

#### 2.3 sort方法

Array.sort()方法是在原数组上对数组元素进行排序，返回排序后的结果，如果调用时不传参数，将按照字母顺序进行排序：

```javascript
var a = ['banan','apple','orange'];
a.sort();
a.join();
"apple,banan,orange"
a
["apple", "banan", "orange"]
```

如果含有未定义的元素，这些元素将被放在数组的末尾
如果要按照别的顺序排序，需要传递给sort方法一个比较函数

```javascript
var a = [2,5,1];
a.sort(function(a,b){
    return a-b;
})
 [1, 2, 5]

var a = [2,5,1];
a.sort(function(a,b){
    return b-a;
})
 [5, 2, 1]
```

#### 2.4 concat

方法Array.concat()能创建并返回一个数组,这个数组包含了调用concat()的原始数组的元素，其后跟随的是concat()的参数。如果其中有些参数是数组,那么它将被展开,其元素将被添加到返回的数组中。但是要注意concat()并不能递归地展开一个元素为数组的数组。下面是一些例子:

```javascript
var a=[1,2,3];
a.concat(4,5)
//[1,2,3.4,5]
a.concat([4,5]);
//返回[1,2,3,4,5]
a.concat([4,5],[6,7])
//[1,2,3,4,5,6,7]
a.concat(4,[5,[6,7]])//返回[1,2,3,4,5,[6,7]]
```

#### 2.5 slice()方法

方法Array.slice()方法返回的是指定的一个数组片段，它的两个参数指定要返回数组的起止点，返回的值是包含第一个参数指定的元素和第二个参数指定元素的上一个元素为止的元素，但是并不包含第二个参数包含的元素。如果只传递一个参数，那么返回从第一个元素到末尾之间的元素，如果参数含有负数，那么从数组末尾开始算，例如，-1指的是数组最后一个元素。例：

```javascript
var a = [1,2,3,4,5]
a.slice(0,3)
 [1, 2, 3]
a.slice(1,-1)
[2, 3, 4]
a.slice(-3,-2)
[3]
```

#### 2.6 splice方法

方法Array.splice()在原数组上插入或删除数组元素，修改原数组。
第一个参数指定了要插入或删除的的元素在数组中的位置。第二个参数指定了要删除的元素个数。如果第二个参数省略，将删除从开始元素到末尾之间的元素，splice返回的是删除了的数组元素数组，如果没有删除，返回空数组：

```javascript
var a = [1,2,3,4,5]
a.splice(4)
[5]
a
[1, 2, 3, 4]

a.splice(1,2)
[2, 3]
a
[1, 4]

a.splice(1,0,'A','B')
[]
a
[1, "A", "B", 4]

a.splice(1,0,[2,3]);
[]
a
[1,[2,3],'A','B']
// splice并不将插入的元素展开
```

#### 2.7 push方法和pop方法

push和pop可以像栈那样使用数组，push可以向数组末尾添加一个或多个数组元素，并返回数组的新长度，pop可以删除数组最后一个元素，返回删除的值，这两个方法都修改原数组：

```javascript
var stack = [];
stack.push(1,2)
2
stack.pop()
2
stack.push(3)
2
stack.pop()
3
stack.push([4,5])
2
stack.pop()
 [4, 5]
stack.pop()
1
stack
[]
```

#### 2.8 unshift和shift方法

和push和pop类似，只不过是在数组头部插入和删除元素

```javascript
var a = []
undefined
a.unshift(1)
1
a.unshift(22)
2
a.shift()
22
a
[1]0: 1length: 1__proto__: Array(0)
a.unshift([4,5])
2
a
 [[4, 5], 1]
a.shift()
[4, 5]
a.shift()
1
a
[]
```

#### 2.9 toString方法

数组的toString将数组每个元素转换为字符串，用逗号拼接，并且没有其他的界定符

```javascript
[1,2,3].toString()
"1,2,3"
["a","b","c"].toString()
"a,b,c"
[1,[2,"c"]].toString()
"1,2,c"
```

注意toString的返回值和无参数的join方法返回值相等：

```javascript
[1,[2,"c"]].join()
"1,2,c"
```