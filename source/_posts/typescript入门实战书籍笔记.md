---
title: typescript入门实战书籍笔记
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2022-09-15 15:22:30
---

TypeScript是一门专为开发大规模JavaScript应用程序而设计的编程语言，是JavaScript的超集，包含了JavaScript现有的全部功能，并且使用了与JavaScript相同的语法和语义。

TypeScript代码不能直接运行，它需要先被编译成JavaScript代码然后才能运行。Type-Script编译器（tsc）将负责把TypeScript代码编译为JavaScript代码

# 第一章 typescript语言基础

### 1.变量

可以用变量来存储和操作数据。当我们操作变量时，实际上操作的是变量对应的存储地址中的数据。

变量名也叫标识符，定义规则：

1. 允许包含字母、下划线、数字和美元符号"$"
2. 允许包含unicode转义序列，如"\u0069\u{6F}"
3. 仅允许使用字母、unicdoe转义序列，下划线，美元符号"$"作为第一个字符
4. 标识符区分大小写
5. 不允许使用保留字作为标识符

变量声明：var, let, const   let和const声明变量具有块级作用域
块级作用域的概念包含了两部分，即块和作用域。变量的作用域指的是该变量的可访问区域，一个变量只能在其所处的作用域内被访问，在作用域外是不可见的。块级作用域中的块指的是“块语句”。块语句用于将零条或多条语句组织在一起。在语法上，块语句使用一对大括号“{}”来表示

### 2.注释

TypeScript支持三种类型的注释：

1. 单行注释
2. 多行注释
3. 区域注释

单行注释：

```ts
// single line comment
```

多行注释:

```ts
/**
 * multi-line comment
 * multi-line comment
 */
```

区域注释：区域注释不是一种新的注释语法，它借助单行注释的语法实现了定义代码折叠区域的功能

```ts
01 //#region 区域描述
02 
03 let x = 0;
04 
05 //#endregion
```

### 3.数据类型

在ECMAScript 2015规范中定义了如下七种数据类型：

1. Undefined
2. Null
3. Boolean
4. Number
5. String
6. Symbol
7. Object

其中，Undefined、Null、Boolean、String、Symbol和Number类型是原始数据类型，Object类型是非原始数据类型。原始数据类型是编程语言内置的基础数据类型，可用于构造复合类型。

#### 1.Undefined

Undefined类型只包含一个值，即undefined。在变量未被初始化时，它的值为undefined。

#### 2.Null

Null类型也只包含一个值，即null。我们通常使用null值来表示未初始化的对象。此外，null值也常被用在JSON文件中，表示一个值不存在。

#### 3.Boolean

Boolean类型包含两个逻辑值，分别是true和false。

#### 4.String

String类型表示文本字符串，它由0个或多个字符构成.
Javascript使用utf-16编码来表示一个字符。utf-16编码以2个字节表示一个编码单元，每个字符使用一个编码单元或者两个编码单元来表示。在底层存储中，字符串是由零个或多个16位无符号整数构成的有序序列。
ECMAScript 2015规定了字符串允许的最大长度为2的53次方-1，该数值也是JavaScript所能安全表示的最大整数。

#### 5.Number

Javascript不详细区分整数、浮点数以及带符号的数字类型。使用双精度的64位浮点数字格式（IEEE 754）来表示数字，因此数字本质上都是浮点数。在该格式中，符号占1位（bit），指数部分占11位，小数部分占52位，共64位。

#### 6.Symbol

Symbol是ECMAScript 2015新引入的原始类型。
Symbol值有一个重要特征，每个Symbol值都是唯一且不可改变的。Symbol值的应用场景是作为对象的属性名。

JavaScript提供了一个全局的“Symbol()”函数来创建Symbol类型的值。我们可以将“Symbol()”函数想象成GUID（全局唯一标识符）的生成器，每次调用“Symbol()”函数都会生成一个完全不同的Symbol值。

#### 7.Object

对象是属性的集合。对象属性使用键值来标识，键值只能为字符串或Symbol值，空字符串也是合法的键值。

### 4.字面量

在计算机科学中，字面量用于在源代码中表示某个固定值。在JavaScript程序中，字面量不是变量，它是直接给出的固定值。

#### 1.Null字面量

Null字面量只有一个，记作null。

#### 2.Boolean字面量

Boolean字面量有两个，分别记作true和false。

#### 3.Number字面量

Number字面量包含4类：

1. 二进制整数字面量：以0b或者0B开头，只包含数字0或1
2. 八进制整数字面量：以0o或者0O开头,只包含数字0到7
3. 十进制数字字面量：一串数字组成，支持整数、小数、科学记数法
4. 十六进制整数字面量：以0x或者0X开头,可以包含数字0至9、小写字母a至f以及大写字母A至F

#### 4.字符串字面量

字符串字面量是使用一对单引号或双引号包围起来的Unicode字符.
字符串字面量中可以包含Unicode转义序列和十六进制转义序列。

#### 5.模版字面量

模板字面量是ECMAScript 2015引入的新特性，它提供了一种语法糖来帮助构造字符串。
模板字符串的基本语法是使用反引号“`”替换了字符串字面量中的单、双引号

```html
 const template = `
<table>
  <tr>
    <th>昵称</th>
    <th>性别</th>
  </tr>
  <tr>
    <td>多米</td>
    <td>女</td>
  </tr>
</table>
12 `;
```

字符串占位符：“${}”

```html
const root = 'https://www.bai.com'
const url = `${root}?a=1`
```

### 5.对象

在JavaScript中，对象属于非原始类型。同时，对象也是一种复合数据类型，它由若干个对象属性构成。对象属性可以是任意数据类型，如数字、函数或者对象等。当对象属性为函数时，我们通常称之为方法。

#### 1.对象字面量

1.数据属性
数据属性由属性名和属性值组成：
{ 
  PropertyName: PropertyValue,
}

对象属性名可以为标识符、字符串字面量和数字字面量，对象属性值可以为任意值

2.存取器属性
一个存取器属性由一个或两个存取器方法组成，分别为get方法和set方法两种。
如果一个属性只定义了get方法而没有定义对应的set方法，那么该属性就成了只读属性。

#### 2.原型对象

每个对象都有一个原型。对象的原型既可以是一个对象，即原型对象，也可以是null值。原型对象又有其自身的原型，对象的原型会形成一条原型链，原型链将终止于null值。
原型能够用来在不同对象之间共享属性和方法，JavaScript中的继承机制也是通过原型来实现的。
原型的作用主要体现在查询对象某个属性或方法时会沿着原型链依次向后搜索，如果直到原型链尽头null值也没有查询到对应属性，那么会返回undefined。

注：原型对象在属性查询和属性设置时起到的作用是不对等的。在查询对象属性时会考虑对象的原型，但是在设置对象属性时不会考虑对象的原型，而是直接修改对象本身的属性值。

### 6.数组

数组是一组有序元素的集合，它使用数字作为元素索引值。数组属于对象数据类型。

#### 1.数组字面量

数组字面量是常用的创建数组的方法。 const colors = ['red', 'green']

#### 2.数组中元素

数组中元素可以是数字，函数或对象，可以是任意类型的值。通过数字索引访问，从0开始，访问越界不会报错，返回undefined

### 7.函数

JavaScript中的函数是“头等函数”，它具有以下特性：

1. 函数可以赋值给变量或对象属性
2. 函数可以作为函数的返回值
3. 函数可以作为另一个函数的参数传递

#### 1.函数声明

```js
function name(param1,param2,...) {
  // body
}
```

#### 2.函数表达式

函数表达式中的函数名是可选的。当没有指定函数名时，该函数也叫作匿名函数。

立即执行的函数表达式：在定义时就立即被调用的函数表达式

定义方式：
```js
;(function () {
 //...
})()
// 或者

;(function () {

}())
```

包围函数表达式的()叫分组运算符，将函数置于分组运算符之内时，该函数即成了函数表达式。

立即执行的函数表达式两个特点：

1. 能够创建新的作用域，函数内部不会对外部产生影响，提供了一定数据封装性
2. 立即执行的函数表达式自身不会对外部作用域产生任何影响，无法通过函数表达式来访问。

#### 3.箭头函数

箭头函数是ECMAScript 2015中新增的特性，用来定义匿名的函数表达式。
箭头函数一定是匿名函数。

```js
(params1, param2, ...) => { // body }
```

箭头函数没有this绑定，使用外层作用域的this绑定。


# 第二章 TypeScript语言进阶

### 1.BigInt

一个新的原始数据类型，属性数值类型的一种。
目前，JavaScript语言中共有以下七种原始数据类型：

1. Undefined
2. Null
3. Boolean
4. String
5. Number
6. BigInt
7. Symbol

BigInt能表示任意精度的整数，尤其是大于2的53次方-1的整数。

#### 1.创建BigInt

1. 使用BigInt字面量
2. 使用BigInt()函数

BigInt字面量的语法是在一个整数后面添加一个小写字母“n”。

```js
const unit = 1n;

const unit1 = BigInt(1) // 1n
```

#### 2.BigInt与Number

BigInt类型的值与Number类型的值可以进行相等关系比较，在进行严格相等比较时，两者用不相等。BigInt类型的值与Number类型的值不允许进行混合数学运算。
可以通过Number()函数将BigInt类型的值转换为Number类型的值，但是会损失精度。

```js
Number(1n) // 1
```

### 2.展开运算符

...expression

#### 1.展开数组字面量

数组字面量中的展开运算符可以应用在任何可迭代对象上。

```js
const firstHalfYearSeasons = ['Spring', 'Summer'];
const seasons = [...firstHalfYearSeasons, 'Fall', 'Winter'];
seasons; // ["Spring", "Summer", "Fall", "Winter"]
```

#### 2.展开对象字面量

对象字面量的展开运算符会将操作数自身可枚举属性复制到当前对象字面量中。

```js
const point2d = {
  x: 0,
  y: 0
}
const point3d = {
  ...point2d,
  z: 0
}

point3d // { x: 0, y: 0, z: 0 }
```

#### 3.展开函数参数

```js
const nums = [3, 1, 4];
const max = Math.max(...nums);
max;  // 4
```

### 3.解构

#### 1.数组解构

赋值运算符右侧为需要解构的数组，赋值运算符左侧是解构赋值的目标，在解构赋值的同时也支持声明新的变量

```js
const point = [0,1]
const [x, y] = point
x // 0
y // 1
```

#### 2.对象解构

赋值运算符右侧为需要解构的对象，赋值运算符左侧是解构赋值的目标，在解构赋值的同时也支持声明新的变量

```js
const point = {x:0, y:1}
const {x, y} = point
x // 0
y // 1
```

### 4.可选链运算符

当尝试访问对象属性时，如果对象的值为undefined或null，那么属性访问将产生错误。为了提高程序的健壮性，在访问对象属性时通常需要检查对象是否已经初始化，只有当对象不为undefined和null时才去访问对象的属性。

#### 1.基础语法

由"?."组成，有以下三种语法：

1. 可选静态属性访问
2. 可选计算属性访问
3. 可选的函数调用或方法调用

可选的静态属性访问：obj?.prop
如果obj的值为null或者undefined，那么表达式的求值结果为undefined，否则为obj.prop

可选计算属性访问: obj?.[expression]
如果obj的值为null或者undefined，那么表达式的求值结果为undefined，否则为obj.[expression]

可选的函数调用或方法调用: fn?.()
如果fn的值为null或者undefined，那么表达式的求值结果为undefined，否则为fn()

#### 2.短路求值

如果可选链运算符左侧操作数的求值结果为undefined或null，那么右侧的操作数不会再被求值，我们将这种行为称作短路求值。

```js
let a1 = 1
let b1 = undefined
b1?.[++a1]
a1 // 1
```

二元逻辑运算符“&&”和“||”也具有短路求值的特性。

### 3.空值合并运算符

语法：a ?? b

当且仅当"??"运算符左侧操作数a的值为undefined或null时，返回右侧操作数b；否则返回左侧操作数a

# 第三章 TypeScript类型基础

### 1.类型注解

:Type

const greeting:string = 'Hello'

TypeScript中的类型注解是可选的，编译器在大部分情况下都能够自动推断出表达式的类型

### 2.类型检查

类型检查是验证程序中类型约束是否正确的过程。
类型检查既可以在程序编译时进行，即静态类型检查；也可以在程序运行时进行，即动态类型检查。TypeScript支持静态类型检查，JavaScript支持动态类型检查。

TypeScript提供了两种静态类型检查模式：

1. 非严格类型检查（默认方式）。

  非严格类型检查是TypeScript默认的类型检查模式。在该模式下，类型检查的规则相对宽松。例如，在非严格类型检查模式下不会对undefined值和null值做过多限制，允许将undefined值和null值赋值给string类型的变量。

2. 严格类型检查。

    {
      "compilerOptions": {
          "strict": true,
      }
    }

    会尽可能地发现代码中的错误，例如，在严格类型检查模式下不允许将undefined值和null值赋值给string类型的变量。

### 3.原始类型

TypeScript中的原始类型包含以下几种：

1. boolean
2. string
3. number
4. bigint
5. symbol
6. undefined
7. null
8. void
9. 枚举类型
10. 字面量类型

#### 1.boolean

TypeScript中的boolean类型对应于JavaScript中的Boolean原始类型。该类型能够表示两个逻辑值：true和false。

```ts
const yes:boolean = true
const no:boolean = false
```

#### 2.string

TypeScript中的string类型对应于JavaScript中的String原始类型。

```ts
const foo:string = 'foo'
const bar:string = `bar,${foo}`
```

#### 3.number

TypeScript中的number类型对应于JavaScript中的Number原始类型。该类型能够表示采用双精度64位二进制浮点数格式存储的数字。

```ts
// 二进制数
const bin:number = 0b1010

// 八进制数
const oct:number = 0o744

// 十进制数
const integer:number = 10
const float:number = 3.14

// 十六进制数
const hex:number = 0xffffff

```

#### 4.bigint

TypeScript中的bigint类型对应于JavaScript中的BigInt原始类型。该类型能够表示任意精度的整数，但也仅能表示整数

```ts
// 二进制数
const bin:number = 0b1010n

// 八进制数
const oct:number = 0o744n

// 十进制数
const integer:number = 10n

// 十六进制数
const hex:number = 0xffffffn

```

#### 5.symbol与unique symbol

TypeScript中的symbol类型对应于JavaScript中的Symbol原始类型。该类型能够表示任意的Symbol值。

```ts
// 自定义Symbol
const key: symbol = Symbol();
// Well-Known Symbol
const symbolHasInstance: symbol = Symbol.hasInstance;
```

字面量能够表示一个固定值。例如，数字字面量“3”表示固定数值“3”.
symbol类型不同于其他原始类型，它不存在字面量形式
symbol类型的值只能通过"Symbol()"和"Symbol.for()"来创建或者直接饮用某个well-known Symbol值。

```ts
const s0: symbol = Symbol();
const s1: symbol = Symbol.for('foo');
const s2: symbol = Symbol.hasInstance;
const s3: symbol = s0;
s0 // Symbol()
s1 // Symbol(foo)
s2 // Symbol(Symbol.hasInstance)
s3 // Symbol()
```

为了能够将一个Symbol值视作表示固定值的字面量，TypeScript引入了“unique symbol”类型.

```ts
const s0: unique symbol = Symbol();
const s1: unique symbol = Symbol.for('s1');
```

“unique symbol”类型的主要用途是用作接口、类等类型中的可计算属性名.
因为如果使用可计算属性名在接口中添加了一个类型成员，那么必须保证该类型成员的名字是固定的，否则接口定义将失去意义。

```ts
const s0: unique symbol = Symbol();
const s1: symbol = Symbol.for('s1');
console.log(s0,s1)
interface foo {
    [s0]: String,
    [s1]: String // A computed property name in an interface must refer to an expression whose type is a literal type or a 'unique symbol' type. 计算属性名称必须引用类型为字面量类型或者'unique symbol'类型
}
```

TypeScript中只允许使用const声明或readonly属性声明来定义“unique symbol”类型的值。

```ts
// 必须使用const声明
const a: unique symbol = Symbol();

interface WithUniqueSymbol {
    // 必须使用readonly修饰符
    readonly b: unique symbol;
}

class C {
   // 必须使用static和readonly修饰符
    static readonly c: unique symbol = Symbol();
}
```

如果使用let或var声明定义“unique symbol”类型的变量，那么将产生错误，因为标识符与Symbol值之间的绑定是可变的


“unique symbol”类型的值只允许使用“Symbol()”函数或“Symbol.for()”方法的返回值进行初始化，因为只有这样才能够“确保”引用了唯一的Symbol值.

```ts
const a: unique symbol = Symbol();
const b: unique symbol = Symbol('desc');

const c: unique symbol = a;
// 错误：a的类型与c的类型不兼容
```

使用相同的参数调用“Symbol.for()”方法实际上返回的是相同的Symbol值
因此，可能出现多个“unique symbol”类型的值实际上是同一个Symbol值的情况。由于设计上的局限性，TypeScript目前无法识别出这种情况.
在设计上，每一个“unique symbol”类型都是一种独立的类型。在不同的“unique symbol”类型之间不允许相互赋值；在比较两个“unique symbol”类型的值时，也将永远返回false.

```ts
const e: unique symbol = Symbol('same');
const f: unique symbol = Symbol('same'); 
if(e === f) { 
  // false
  console.log('equal')
}
// 注：
const e: unique symbol = Symbol.for('same');
const f: unique symbol = Symbol.for('same')
// 此时e和f相等，打印equal,但Symbol('same')和Symbol('same')；
// Symbol('same')和Symbol.for('same')不相等，不知道为啥，应该是都不相等的，但运行代码能打印equal
```

“unique symbol”类型是symbol类型的子类型，可以将“unique symbol”类型赋值给symbol类型。

```ts
const f: unique symbol = Symbol.for('same')
const g:symbol = f
```


#### 6.Nullable

TypeScript中的Nullable类型指的是值可以为undefined或null的类型。

1. undefined类型：只包含一个值，即undefined

```ts
const foo:undefined = undefined
```

2. null类型：只包含一个值，即null

```ts
const foo:null = null
```

3. strictNullChecks,即严格的null检查模式，同时作用于undefined类型和null类型检查
默认不启用该选项，在不启用时允许将undefined和null值赋值给string类型等其他类型，如下：

```ts
/**
 * strictNullChecks:false
 */
let m1:boolean = undefined
let m2:string = undefined
let m3:number = undefined
let m4:bigint = undefined
let m5:symbol = undefined
let m6:null = undefined
let m7:undefined = undefined

let n1:boolean = null
let n2:string = null
let n3:number = null
let n4:bigint = null
let n5:symbol = null
let n6:null = null
let n7:undefined = null
```

当启用strictNullChecks编译选项时，undefined和null值不能再赋值给不相关的类型，undefined只能赋值给undefined，null值也只能赋值给null。
但是undefined值和null值允许赋值给顶端类型，同时undefined值也允许赋值给void类型

```ts
let m1:void = undefined

let m2:any = undefined
let m3:unknown = undefined

let n2:any = null
let n3:unknown = null

// null和undefined是两个不同的类型
let foo:undefined = null // 编译错误 null不能赋值给undefined
let bar:null = undefined // 编译错误
```

#### 7.void

void类型表示某个值不存在，该类型用作函数的返回值类型。

除了将void类型作为函数返回值类型外，在其他地方使用void类型是无意义的。

```ts
function log(message:string):void {
  console.log(message)
}
```

启用strictNullChecks时只允许将undefined赋值给void

```ts
/**
 * strictNullChecks:true
 */
function foos():void {
    return undefined
}

function foo1():void {
    return null //wrong: Type 'null' is not assignable to type 'void'.
}

/**
 * strictNullChecks:false
 */
// 正确
function foos():void {
  return undefined
}
// 正确
function foo1():void {
  return null
}
```

### 4.枚举类型

