---
title: javascript-第八章-对象
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-13 17:08:42
---


## 第八章-对象

### 1. 对象和属性

对象是一种复合数据类型，将多个数据值存储在一个单元中，并使用名字来存取这些值。
解释对象的另一种方式就是，对象是一个无序的属性集合，每个属性都有自己的名字和值，存储在对象中已命名的值既可以是数字字符串也可以是对象。

#### 1.1 对象的创建

对象是由运算符new创建的，在这个运算符之后必须有用于初始化对象的构造函数名，可以用如下代码创建空对象（没有属性的对象）：

```javascript
var o = new Object();
```

javascript还支持内部构造函数，例如： var now  = new Date();
对象直接量定义了另一种初始化对象的方式。例如：

```javascript
var object = {
    x:0,
    y:0,
    radius:10
}
```

#### 1.2 属性的设置和查询

通常使用“.”运算符来存取对象的属性。位于“.”运算符左边的是对象的引用，右边的是对象的属性，是一个标识符。

```javascript
var book = new Object();
book.title = 'javascript高级程序设计';//设置属性
book.title ; //属性查询
```

#### 1.3 属性的枚举

可以通过for/in来遍历对象的属性：

```javascript
function getObjNames(obj){
    var names = “”;
    for(var name in obj) names+= name ;
}
```

for/in循环属性并没有特定的顺序，虽然能枚举出用户定义的属性，但不能枚举出某些预定义的属性和方法

#### 1.4 未定义的属性

如果要读取一个未定义的属性，那么读取的值是undefined.
可以用运算符delete来删除对象的属性：
delete book.title;
删除一个属性不仅仅是将属性值设置为undefined,而是真正从该对象中移除该属性。用for/in循环可以证明两者区别，它只能枚举出来被设定为undefined的属性，不能枚举已删除的属性。

### 2. 构造函数

在javascript中，使用new运算符和预定义的构造函数（Date(),Object(),Function()等）可以创建并初始化一个对象。

要创建已经定义了属性的对象，需要编写一个构造函数在新的对象中创建并初始化这些属性。构造函数是具有2个特性的javascript函数：
+ 由new运算符调用
+ 传递给它的是对一个新创建的空对象的引用，将该引用作为关键字this的值，而且还要对新创建的对象进行初始化

如下例子说明如何定义并调用Rectangle对象的构造函数
Rectangle对象的构造函数

```javascript
/定义构造函数
//注意如何初始化this引用的对象
function Rectangle(w,h){
    this.width = w;
    this.height = h;
}
//调用构造函数创建2个Rectangle对象
//注意把宽和高传递给构造函数，这样就能初始化对象
var rect1 = new Rectangle(2,4);
var rect2 = new Rectangle(8,9);
```

构造函数只是初始化了对象，并不返回这个对象。构造函数通常没有返回值，只是初始化由this值传递进来的对象。
但是，构造函数可以返回一个对象值，如果这样做，被返回的对象就成了new表达式的值，this所引用的对象就被丢弃了。

### 3. 方法

所谓方法（method）就是通过对象调用的javascript函数。函数就是数值，可以将函数赋值给任何变量，设置对象的属性。如果一个函数f和一个对象o，可以定义一个名为m的方法：o.m = f;
定义m方法之后可以通过o.m()调用它。

方法有一个重要的属性，即函数主体内部，关键字this变成了调用该方法的对象。例如，在o.m()中，主体可以使用关键字this来引用对象o.

任何一个用作方法的函数都会得到一个额外的实际参数，即调用该方法的对象。

由于方法通常是对对象执行某种操作，要表达函数作用于对象，最好采用方法的调用的方法。

```javascript
rect.setSize(width,height);
setRectSize(rect, width, height);
```

虽然这两行代码都是对对象做同样的操作，但第一行采用方法的调用语法。表达rect是操作的焦点。

虽然有区别地对特函数和方法比较有用，但实际上它们之间的差别井没有最初时那么大了。
回忆一下，函数是储存在变量中的值，而那个变量也不过是全局对象的一个属性。因此，当你调用一个函数时，实际上调用的是全局对象的一个方法。
在这样的函数中，关键字this引用的是全局对象。所以在函数和方法之间井设有件么技术上的差别。
真正的差别存在于设计和目的上，方法是用来对上this对象进行操作的，而函数通常是独立的，并不需要使用this对象。

```javascript
// 例8-2方法的定义和调用
function compute_area(){
    return this.width * this.height;
}
var page = new Rectangle(9,88);
page.area = compute_area; //通过把函数赋予对象的属性，来定义一个方法。
var a = page.area();
```

在例8-2中有一个明显的缺点，那就是在调用rect对象的方法area()之前，必须先将该方法赋给rect对象的一个属性。

```javascript
例8-3 用构造函数定义方法
//首页定义一些函数，它们将被用作方法。
function Rectangle_area() { return this.width * this.height; }
function Rectangle_perimeter() { return 2*this.width + 2*this.height; }
function Rectangle_set_size(w,h) { this.width = w; this.height = h; }

//为Rectangle对象定义一个构造函数，不仅要初始化属性还要给方法赋值。
function Rectangle(w,h){
    //初始化对象的属性
    this.width = w;
    this.height = h;

    //定义对象的方法
    this.area = Rectangle;
    this.perimeter = Rectangle_perimeter;
    this.set_size = Rectangle_set_size;
}
//一旦创建Rectangle对象，就可以直接调用它的方法
var r = new Rectangle(2,2);
var a = r.area();
var p = r.perimeter();
```

例8-3说明的方法还是有缺点，构造函数Rectangle()要对它所有属性都进行设置。由于每个属性都占用一定内存空间，所以给每个Rectangle类添加方法，内存占用就会增加。

### 4.原型对象和继承

我们已经知道，用构造函数把方法赋予它要初始化的对象效率低下。
javascript对象都继承原型对象的属性。每个对象都有原型对象，原型对象的所有属性都是以它为原型的对象的属性。也就是说，每个对象都继承原型对象的所有属性。

一个对象的原型是由创建并初始化该对象的构造函数定义的。
javascript中所有函数都有prototype属性，它引用了一个对象。虽然原型对象初始化是空的，但你在其中定义的任何属性都会被该构造函数创建的所有对象继承。
因为原型对象的属性被一个类的所有对象共享，所有通常只用它们来定义类中所有相同的属性。这使得原型对象适用于方法定义，另外原型属性还适合于那些具有常量值的属性定义。

#### 4.1 原型和内部类

不只是用户定义的类具有原型对象，像String和Date这样的内部类同样具有原型对象，也可以给它们赋值。

```javascript
String.prototype.endsWith = function(c){
    return (c == this.charAt(this.length - 1))
}
var message = “hello world”;
message.endsWith(‘h’); //false
message.endsWith(‘d’); //true
```

### 5. 面向对象的javascript

javascript采取以原型对象为基础的继承机制，而不是采取类为基础的继承机制。

#### 5.1 实例的属性

每个对象都有它自己单独的属性副本。如果有10个给定的类的对象，那么每个实例属性就有10个副本。
在默认情况下，javascript对象的属性都是实例属性，但是为了更真实的模拟面向对象的设计语言，我们说javascript的实例属性都是在对象中用构造函数创建的或初始化的属性。

#### 5.2 实例方法

实例方法和实例属性十分相似，只不过是方法而不是数值。实例方法由特定方法和实例对象调用的。
实例方法使用了this来引用它们要操作的对象或实例。虽然类的任何实例都可以调用实例方法，但是并不意味着每个对象都像实例属性那样含有自己专有的方法副本。相反，每个实例方法都是由类的所有实例所共享的。
在javascript中，给类定义一个实例方法，是通过把构造函数的原型对象的一个属性设置为函数值来实现的。

#### 5.3 类属性

在Java中，类属性是和类相关联的的变量。而不是和类的每个实例相关联的变量。无论类创建多少个实例，每个类属性就只有一个副本。就像实例属性是通过实例存取一样，类属性是通过类存取的。在javascript中，Number.MAX_VALUE就是类属性的一个例子，因为MAX_VALUE就是通过类Number存取的.

#### 5.4 类方法

类方法是一个与类关联的一起的方法，而不是和类的实例关联在一起的方法。要调用类方法就必须使用类本身，而不能使用类的实例。方法Date.parse()就是一个类方法。和类属性一样，类方法是全局性的。
因为类方法不是对特定对象进行操作的所以类方法更容易被认为是由类调用的函数。在javascript中，要定义一个类方法，只需要用合适的函数作为构造函数的属性即可。


#### 5.5 例子，类Circle()

```javascript
// 定义构造函数本身
function Circle(radius){
    this.r = radius;
    //r是构造函数定义并初始化的一个实例属性
}
Circle.PI = 3.14159;//定义类属性，是构造函数的一个属性

function Cricle_area(){
    return Circle.PI * this.r * this.r;
}

Circle.prototype.area = Cricle_area();//通过把函数赋给构造函数的原型对象来定义一个实例方法。

function Circle_max(a,b){
    if(a.r > b.r) return a;
    else return b;
}
Circle.max = Circle_max;//将函数赋给构造函数的属性，使之成为类方法

var c = new Circle(1.0);//创建Circle类的一个实例
c.r = 2.2; //修改类实例属性值
var a = c.crea(); //调用实例方法
var d = new Circle(1.2);
var bigger = Circle.max(c,d); //调用类方法
```

#### 5.6 例子，类Complex复数

```javascript
/*
复数就是一个实数和一个虚数的和
*/
/**
* 定义类的第一步就是定义该类的构造函数
* 这个构造函数要初始化对象的所有实例属性
* 这些属性是核心的状态变量，是它们使每个类实例不同
*/
function Complex(real, imaginary){
    this.x = real;//复数的实数部分
    this.y = imaginary; //复数的虚数部分
}
/**
* 定义类的第二部是在构造函数的原型对象定义他的实例方法（或属性）
* 该对象定义的任何属性都被类的实例所继承
* 返回复数的大小
*/
Complex.prototype.maginary = function(){
    return Math.sqrt(this.x * this.x,this.y * this.y);
}
//返回复数的相反数
Complex.prototype.negative = function(){
    return new Complex(-this.x, -this.y);
}
//将Complex对象转化成一个字符串
Complex.prototyoe.toString = function(){
    return "{"+ this.x+','+this.y+"}";
}

// 返回一个复数的实数部分
Complex.prototyoe.valueOf = function(){
    return this.x;
}

/**
* 定义类的第二部是定义类方法
* 常量和其他必要属性作为构造函数的属性而不是构造函数原型对象的属性
* 注意，类方法没有使用this关键字，因为它们只对实际参数进行操作
*/
Complex.add = function(a,b){
    return new Complex(a.x+b.x,a.y+b.y);
}

Complex.subtract = function(a,b){
    return new Complex(a.x-b.x,a.y-b.y);
}
/**
* 下面是预定义复数
* 被定义为类属性，可以被用作常量，实际上并不是只读的
*/
Complex.zero = new Complex(0,0);
Complex.one = new Complex(1,0);
```


#### 5.7 超类和子类

在javascript中，类Object是最通用的类，其他所有类都是专用化了的类，或者说是Object的子类。
另一种解释就是Object是所有内部类的超类。所有类都继承类Object的基本方法。
对象从他的构造函数的原型对象中继承属性，原型对象本身就是一个对象，由构造函数Object()创建。这就意味着原型对象继承了Object.prototype属性，因此类Complex就继承了Complex.prototype的属性，而后者又继承了Object.prototype属性。因此，Complex继承了两个对象的属性，在Complex中查询某个属性时，首先查询这个对象本身，如果没有查询到，就查询Complex.prototype的对象，如果还未查询到，就查询Object.prototype对象。

### 6. 对象的属性和方法

在javascript中，所有对象都是Object类创建而来，虽然一些专用的类比如String和用户自定义的类Complex都定义了自己的属性和方法，但是都支持Object类定义的属性和方法。

#### 6.1 constructor属性

每个对象都有constructor属性，它引用的是初始化该对象的构造函数，例如用构造函数Complex创建类一个对象o，那么o.constructor引用的就是Complex，

```javascript
var o = new Complex(1,1);
o.constructor == Complex;
```

#### 6.2 toString方法

方法toString没有任何实际参数，它返回的是一个字符串，该方法返回的是调用它的对象的类型或值。当使用“+”运算符把一个字符串和一个对象连接一起或把一个对象传递给alert()或document.write()时，会调用toString方法。

由类Object定义的默认的toString方法揭示了有关内置对象的内部类型信息。	
默认的toString方法返回的字符串形式总是[object class]，class是对象的内部类型，通常对应于该对象的构造函数名。例如，Array对像的class值是Array，Function对象的class值是“Function”，Date对象的class值是“Date”，内部Math对象的class值是“Math”，所有Error对象的的class值是“Error”，用户定义的对象（例如Complex）的class值是“Object”。

#### 6.3 toLocaelString方法

除了toString外，Object类还定义了toLocaleString方法，该方法返回该对象局部化的字符串表示。Object类默认定义的toLocaelString方法，本身并不做任何局部化，返回值和toString一样。但是Object的子类可以定义自己的toLocaleString方法，Array，Number,Date都定义了自己的toLocaelString方法。

```javscript
let now = new Date();
now.toString()
"Fri Jan 08 2021 14:05:09 GMT+0800 (中国标准时间)"
now.toLocaleString()
"2021/1/8 下午2:05:09"

let num = new Number(1);
num.toString()
"1"
num.toLocaleString()
"1"
```

#### 6.4 valueOf方法

valueOf方法和toString方法非常相似，当需要把对象转化为非字符串之外的原始类型（通常是数字）时，就需要调用它。这个函数返回的是能代表this关键字所引用的对象的值的类型。

由于对象没有定义为原始类型的值，所以大多数对象都没有等价的原始值，因此由Object定义的valueOf方法不执行任何转换，只是返回调用它的对象。像Number和Boolean这样的类具有明显的原始等价值。

有时你可以定义一个合理的原始等价类型的类，需要为类定义一个valueOf方法，例如Complex，就定义了一个valueOf方法，返回实数部分。定义valueOf方法时，当某些环境中，当执行对象转字符串时，方法valueOf的优先级比toString高，这样当你定义了一个valueOf方法时，如果想转为字符串需要明确的调用toString方法，以Complex为例：

```javascript
var c = new Complex(3,3);
alert(“c”+c);//调用valueOf方法显示“c=3”
alert(“c”+c.toString);//显示“c={3,3}”
```

#### 6.5 hasOwnProperty

如果对象局部定义了一个非继承的属性，属性名是由字符串实际参数指定的，那么该值返回true，否则返回false，例：

```javascript
var o = new Object();
o.hasOwnProperty(“unde”);//false
o.hasOwnProperty(“toString”);//false,toString 是一个继承属性
Math.hasOwnProperty(“cos”);//true
```

#### 6.6 propertyIsEnumerable()方法

如果对象定义了一个属性，由字符串实际参数指定的，而且该属性可以用for/in遍历出来，那么该方法返回true，否则返回false，例如：

```javascript
var o = {x:1};
o.propertyIsEnumerable(“x”); //true
o.propertyIsEnumerable(“y”); //false
o.propertyIsEnumerable(“valueOf”); //false
```

ECMAScript标准规定该方法只考虑对象直接定义的属性，不考虑对象的继承属性。

#### 6.7 isPrototypeOf方法

如果调用对象是实际参数指定对象的原型对象，那么返回true，否则返回false，该方法用途和constructor属性相似：

```javascript
var  o = new Object();
Object.prototype.isPrototypeOf(o);//true  o.constructor == Object

Function.prototype.isPrototypeOf(Object); //true  Object.constructor == Function
```

