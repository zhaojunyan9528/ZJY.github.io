---
title: TypeScript入门实战书籍笔记
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2022-09-15 15:22:30
---

TypeScript是一门专为开发大规模JavaScript应用程序而设计的编程语言，是JavaScript的超集，包含了JavaScript现有的全部功能，并且使用了与JavaScript相同的语法和语义。

TypeScript代码不能直接运行，它需要先被编译成JavaScript代码然后才能运行。Type-Script编译器（tsc）将负责把TypeScript代码编译为JavaScript代码

## 第一章 TypeScript语言基础

### 1.1 变量

可以用变量来存储和操作数据。当我们操作变量时，实际上操作的是变量对应的存储地址中的数据。

变量名也叫标识符，定义规则：

1. 允许包含字母、下划线、数字和美元符号"$"
2. 允许包含unicode转义序列，如"\u0069\u{6F}"
3. 仅允许使用字母、unicdoe转义序列，下划线，美元符号"$"作为第一个字符
4. 标识符区分大小写
5. 不允许使用保留字作为标识符

变量声明：var, let, const   let和const声明变量具有块级作用域
块级作用域的概念包含了两部分，即块和作用域。变量的作用域指的是该变量的可访问区域，一个变量只能在其所处的作用域内被访问，在作用域外是不可见的。块级作用域中的块指的是“块语句”。块语句用于将零条或多条语句组织在一起。在语法上，块语句使用一对大括号“{}”来表示

### 1.2 注释

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

### 1.3 数据类型

在ECMAScript 2015规范中定义了如下七种数据类型：

1. Undefined
2. Null
3. Boolean
4. Number
5. String
6. Symbol
7. Object

其中，Undefined、Null、Boolean、String、Symbol和Number类型是原始数据类型，Object类型是非原始数据类型。原始数据类型是编程语言内置的基础数据类型，可用于构造复合类型。

#### 1.3.1 Undefined

Undefined类型只包含一个值，即undefined。在变量未被初始化时，它的值为undefined。

#### 1.3.2 Null

Null类型也只包含一个值，即null。我们通常使用null值来表示未初始化的对象。此外，null值也常被用在JSON文件中，表示一个值不存在。

#### 1.3.3 Boolean

Boolean类型包含两个逻辑值，分别是true和false。

#### 1.3.4 String

String类型表示文本字符串，它由0个或多个字符构成.
Javascript使用utf-16编码来表示一个字符。utf-16编码以2个字节表示一个编码单元，每个字符使用一个编码单元或者两个编码单元来表示。在底层存储中，字符串是由零个或多个16位无符号整数构成的有序序列。
ECMAScript 2015规定了字符串允许的最大长度为2的53次方-1，该数值也是JavaScript所能安全表示的最大整数。

#### 1.3.5 Number

Javascript不详细区分整数、浮点数以及带符号的数字类型。使用双精度的64位浮点数字格式（IEEE 754）来表示数字，因此数字本质上都是浮点数。在该格式中，符号占1位（bit），指数部分占11位，小数部分占52位，共64位。

#### 1.3.6 Symbol

Symbol是ECMAScript 2015新引入的原始类型。
Symbol值有一个重要特征，每个Symbol值都是唯一且不可改变的。Symbol值的应用场景是作为对象的属性名。

JavaScript提供了一个全局的“Symbol()”函数来创建Symbol类型的值。我们可以将“Symbol()”函数想象成GUID（全局唯一标识符）的生成器，每次调用“Symbol()”函数都会生成一个完全不同的Symbol值。

#### 1.3.7 Object

对象是属性的集合。对象属性使用键值来标识，键值只能为字符串或Symbol值，空字符串也是合法的键值。

### 1.4 字面量

在计算机科学中，字面量用于在源代码中表示某个固定值。在JavaScript程序中，字面量不是变量，它是直接给出的固定值。

#### 1.4.1 Null字面量

Null字面量只有一个，记作null。

#### 1.4.2 Boolean字面量

Boolean字面量有两个，分别记作true和false。

#### 1.4.3 Number字面量

Number字面量包含4类：

1. 二进制整数字面量：以0b或者0B开头，只包含数字0或1
2. 八进制整数字面量：以0o或者0O开头,只包含数字0到7
3. 十进制数字字面量：一串数字组成，支持整数、小数、科学记数法
4. 十六进制整数字面量：以0x或者0X开头,可以包含数字0至9、小写字母a至f以及大写字母A至F

#### 1.4.4 字符串字面量

字符串字面量是使用一对单引号或双引号包围起来的Unicode字符.
字符串字面量中可以包含Unicode转义序列和十六进制转义序列。

#### 1.4.5 模版字面量

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

### 1.5 对象

在JavaScript中，对象属于非原始类型。同时，对象也是一种复合数据类型，它由若干个对象属性构成。对象属性可以是任意数据类型，如数字、函数或者对象等。当对象属性为函数时，我们通常称之为方法。

#### 1.5.1 对象字面量

1.数据属性
数据属性由属性名和属性值组成：
{
  PropertyName: PropertyValue,
}

对象属性名可以为标识符、字符串字面量和数字字面量，对象属性值可以为任意值

2.存取器属性
一个存取器属性由一个或两个存取器方法组成，分别为get方法和set方法两种。
如果一个属性只定义了get方法而没有定义对应的set方法，那么该属性就成了只读属性。

#### 1.5.2 原型对象

每个对象都有一个原型。对象的原型既可以是一个对象，即原型对象，也可以是null值。原型对象又有其自身的原型，对象的原型会形成一条原型链，原型链将终止于null值。
原型能够用来在不同对象之间共享属性和方法，JavaScript中的继承机制也是通过原型来实现的。
原型的作用主要体现在查询对象某个属性或方法时会沿着原型链依次向后搜索，如果直到原型链尽头null值也没有查询到对应属性，那么会返回undefined。

注：原型对象在属性查询和属性设置时起到的作用是不对等的。在查询对象属性时会考虑对象的原型，但是在设置对象属性时不会考虑对象的原型，而是直接修改对象本身的属性值。

### 1.6 数组

数组是一组有序元素的集合，它使用数字作为元素索引值。数组属于对象数据类型。

#### 1.6.1 数组字面量

数组字面量是常用的创建数组的方法。 const colors = ['red', 'green']

#### 1.6.2 数组中元素

数组中元素可以是数字，函数或对象，可以是任意类型的值。通过数字索引访问，从0开始，访问越界不会报错，返回undefined

### 1.7 函数

JavaScript中的函数是“头等函数”，它具有以下特性：

1. 函数可以赋值给变量或对象属性
2. 函数可以作为函数的返回值
3. 函数可以作为另一个函数的参数传递

#### 1.7.1 函数声明

```js
function name(param1,param2,...) {
  // body
}
```

#### 1.7.2 函数表达式

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

#### 1.7.3 箭头函数

箭头函数是ECMAScript 2015中新增的特性，用来定义匿名的函数表达式。
箭头函数一定是匿名函数。

```js
(params1, param2, ...) => { // body }
```

箭头函数没有this绑定，使用外层作用域的this绑定。

## 第二章 TypeScript语言进阶

### 2.1 BigInt

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

#### 2.1.1 创建BigInt

1. 使用BigInt字面量
2. 使用BigInt()函数

BigInt字面量的语法是在一个整数后面添加一个小写字母“n”。

```js
const unit = 1n;

const unit1 = BigInt(1) // 1n
```

#### 2.1.2 BigInt与Number

BigInt类型的值与Number类型的值可以进行相等关系比较，在进行严格相等比较时，两者用不相等。BigInt类型的值与Number类型的值不允许进行混合数学运算。
可以通过Number()函数将BigInt类型的值转换为Number类型的值，但是会损失精度。

```js
Number(1n) // 1
```

### 2.2 展开运算符

...expression

#### 2.2.1 展开数组字面量

数组字面量中的展开运算符可以应用在任何可迭代对象上。

```js
const firstHalfYearSeasons = ['Spring', 'Summer'];
const seasons = [...firstHalfYearSeasons, 'Fall', 'Winter'];
seasons; // ["Spring", "Summer", "Fall", "Winter"]
```

#### 2.2.2 展开对象字面量

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

#### 2.2.3 展开函数参数

```js
const nums = [3, 1, 4];
const max = Math.max(...nums);
max;  // 4
```

### 2.3 解构

#### 2.3.1 数组解构

赋值运算符右侧为需要解构的数组，赋值运算符左侧是解构赋值的目标，在解构赋值的同时也支持声明新的变量

```js
const point = [0,1]
const [x, y] = point
x // 0
y // 1
```

#### 2.3.2 对象解构

赋值运算符右侧为需要解构的对象，赋值运算符左侧是解构赋值的目标，在解构赋值的同时也支持声明新的变量

```js
const point = {x:0, y:1}
const {x, y} = point
x // 0
y // 1
```

### 2.4 可选链运算符

当尝试访问对象属性时，如果对象的值为undefined或null，那么属性访问将产生错误。为了提高程序的健壮性，在访问对象属性时通常需要检查对象是否已经初始化，只有当对象不为undefined和null时才去访问对象的属性。

#### 2.4.1 基础语法

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

#### 2.4.2 短路求值

如果可选链运算符左侧操作数的求值结果为undefined或null，那么右侧的操作数不会再被求值，我们将这种行为称作短路求值。

```js
let a1 = 1
let b1 = undefined
b1?.[++a1]
a1 // 1
```

二元逻辑运算符“&&”和“||”也具有短路求值的特性。

### 2.5 空值合并运算符

语法：a ?? b

当且仅当"??"运算符左侧操作数a的值为undefined或null时，返回右侧操作数b；否则返回左侧操作数a

## 第三章 TypeScript类型基础

### 3.1 类型注解

:Type

const greeting:string = 'Hello'

TypeScript中的类型注解是可选的，编译器在大部分情况下都能够自动推断出表达式的类型

### 3.2 类型检查

类型检查是验证程序中类型约束是否正确的过程。
类型检查既可以在程序编译时进行，即静态类型检查；也可以在程序运行时进行，即动态类型检查。TypeScript支持静态类型检查，JavaScript支持动态类型检查。

TypeScript提供了两种静态类型检查模式：

1. 非严格类型检查（默认方式）。

  非严格类型检查是TypeScript默认的类型检查模式。在该模式下，类型检查的规则相对宽松。例如，在非严格类型检查模式下不会对undefined值和null值做过多限制，允许将undefined值和null值赋值给string类型的变量。

2. 严格类型检查

```json
{
  "compilerOptions": {
      "strict": true,
  }
}
```

会尽可能地发现代码中的错误，例如，在严格类型检查模式下不允许将undefined值和null值赋值给string类型的变量。

### 3.3 原始类型

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

#### 3.3.1 boolean

TypeScript中的boolean类型对应于JavaScript中的Boolean原始类型。该类型能够表示两个逻辑值：true和false。

```ts
const yes:boolean = true
const no:boolean = false
```

#### 3.3.2 string

TypeScript中的string类型对应于JavaScript中的String原始类型。

```ts
const foo:string = 'foo'
const bar:string = `bar,${foo}`
```

#### 3.3.3 number

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

#### 3.3.4 bigint

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

#### 3.3.5 symbol与unique symbol

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

#### 3.3.6 Nullable

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

#### 3.3.7 void

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

### 3.4 枚举类型

枚举类型由0个或多个枚举成员构成，每个枚举成员都是命名的常量。
枚举类型是ts中的原始类型，由enum关键字定义。枚举类型分为：

1. 数值型枚举
2. 字符串枚举
3. 异构型枚举

#### 3.4.1 数值型枚举

由一组命名的数值常量构成,是nunber类型的子类型

```ts
enum Direction {
  Up, // 0
  Right, // 1
  Down, // 2
  Left // 3
}
const direction: Direction = Direction.Up
```

如果在定义枚举时没有设置枚举成员的值，那么TypeScript将自动计算枚举成员的值。根据TypeScript语言的规则，第一个枚举成员的值为0，其后每个枚举成员的值等于前一个枚举成员的值加1.

在定义数值型枚举时，可以为一个或多个枚举成员设置初始值。对于未指定初始值的枚举成员，其值为前一个枚举成员的值加1.

```ts
enum Direction {
  Up = 1, // 1
  Right, // 2
  Down = 10, // 10
  Left // 11
}
```

枚举类型时number类型的子类型，所以可以将数值型枚举类型赋值给number类型：

```ts
const num: number = Direction.Up
```

number类型的值也可以赋值给枚举类型，即使不在枚举类型枚举成员列表中也不会出错。

```ts
const d1: Direction = 1
const d2: Direction = 100
```

#### 3.4.2 字符串枚举

字符串枚举类型成员必须由字符串字面量或另一个字符串枚举成员来初始化，不存在自增行为。

```ts
enum Direction {
  Up = 'up',
  Right = 'right',
  Down = 'down',
  Left = 'left',

  U = Up,
  R = Right,
  D = Down,
  L = Left
}
```

字符串枚举类型是字符串的子类型，因此允许将字符串枚举类型赋值给string类型。

```ts
const str: string = Direction.Up
```

但是反过来将string类型赋值给字符串枚举类型，将产生编译错误。

#### 3.4.3 异构型枚举

TypeScript允许在一个枚举中同时定义数值型枚举成员和字符串枚举成员，我们将这种类型的枚举称作异构型枚举。不推荐使用

```ts
enum Color {
  Black = 0,
  White = 'white'
}
```

在定义异构型枚举时，不允许使用计算的值作为枚举成员的初始值。

```ts
enum Color {
  Black = 0 + 0, // 在具有字符串值成员的枚举中不允许计算值。
  White = 'white'
}
```

在异构型枚举中，必须为字符串枚举成员之后的数值型枚举成员定义初始值。

```ts
enum ColorA {
  Black,
  White = '1',
}
enum ColorB {
  White = '1',
  Black // wrong: Enum member must have initializer.
}
```

#### 3.4.4 枚举成员映射

不论哪种类型枚举，都可以通过枚举成员名去访问枚举成员值。

```ts
enum ColorA {
    Black,
    White = '1',
}
console.log(ColorA.Black) // 0
console.log(ColorA.White) // '1'
```

对于数值型枚举，不仅可以通过枚举成员名访问枚举成员值，还可以通过枚举成员值去获取枚举成员名。

```ts
enum Bool {
    True = 0,
    False = 1
}

console.log(Bool.True) // 0
console.log(Bool.False) // 1
console.log(Bool[Bool.True]) // 'True'
console.log(Bool[Bool.False]) // 'False'
console.log(Bool[0]) // 'True'
console.log(Bool[1]) // 'False'
```

对于字符串枚举和异构型枚举，则不能够通过枚举成员值去获取枚举成员名.

#### 3.4.5 常量枚举成员和计算枚举成员

每个枚举成员都有一个值，根据枚举成员值的定义可以将枚举成员划分为以下两类：

1. 常量枚举成员
2. 计算枚举成员

常量枚举表达式的具体规则如下：

  常量枚举表达式可以是数字字面量、字符串字面量和不包含替换值的模板字面量。
  常量枚举表达式可以是对前面定义的常量枚举成员的引用
  常量枚举表达式可以是用分组运算符包围起来的常量枚举表达式。
  常量枚举表达式中可以使用一元运算符“+”“-”“~”，操作数必须为常量枚举表达式
  常量枚举表达式中可以使用二元运算符“+”“-”“*”“**”“/”“%”“<<”“>>”“>>>”“&”“|”“^”，两个操作数必须为常量枚举表达式。

```ts
enum Foo {
  A = 0,
  B = 'B',
  C = `C`,
  D = A,

  E = -1, // 一元运算符
  F = 12 + 3, // 二元运算符
  G = (1/1) * 3 // 分组运算符
}
```

字面量枚举成员是常量枚举成员的子集，字面量枚举成员是指满足下列条件之一的枚举成员，具体条件如下：

  枚举成员没有定义初始值
  枚举成员的初始值为数字字面量、字符串字面量和不包含替换值的模板字面量。
  枚举成员的初始值为对其他字面量枚举成员的引用

除常量枚举成员之外的其他枚举成员都属于计算枚举成员。

```ts
enum Foo {
  A = 'A'.length,
  B = Math.pow(2, 3)
}
```

#### 3.4.6 联合枚举类型

当枚举类型中的成员全是常量枚举成员时，该枚举类型成了联合枚举类型。

联合枚举类型中的枚举成员除了能表示一个常量外，还能够表示一种类型，即联合枚举成员类型。
联合枚举成员类型是联合枚举类型的子类型，因此可以将联合枚举成员类型赋值给联合枚举类型。

```ts
enum Direction {
    Up,
    Right,
    Down,
    Left
}
const up: Direction.Up = Direction.Up
console.log(up) // 0
const dir: Direction = up
console.log(dir) // 0
```

#### 3.4.7 const枚举类型

枚举类型是ts对js的扩展，js本身并不支持枚举类型。在编译时，ts会将枚举类型编译为js对象。

const枚举类型将在编译阶段被完全删除，并且在使用了const枚举类型的地方将const枚举成员的值内联到代码中。

const枚举类型使用“const enum”关键字定义，示例如下：

```ts
const enum Direction {
    Up,
    Right,
    Down,
    Left
}
const directions = [
    Direction.Up,
    Direction.Right,
    Direction.Down,
    Direction.Left
]
```

编译后:

```js
"use strict";
const directions = [
  0 /* Up */,
  1 /* Right */,
  2 /* Down */,
  3 /* Left */
];
``` 

### 3.5 字面量类型

每一个字面量类型都只有一个可能的值，即字面量本身。

#### 3.5.1 boolean字面量类型

1. true字面量类型
2. false字面量类型

原始类型boolean等同于由true字面量类型和false字面量类型构成的组合类型。
true字面量类型只能接受true值；同理，false字面量类型只能接受false值，示例如下：

```ts
const a: true = true
cosnt b: false = false
```

boolean字面量类型是boolean类型的子集，因此可以将boolean字面量类型赋值给boolean类型。

```ts
const a: true = true;
const b: false = false;
let c: boolean;
c = a;
c = b;
 ```

#### 3.5.2 string字面量类型

字符串字母量和模版字面量都可以创建字符串。字符串字面量和不带参数的模版字面量可以作为string字面量。

```ts
const a: 'hello' = 'hello'
const b: `world` = `world`
let c: string
c = a
c = b
```

string字面量类型是string类型的子类型，因此可以将string字面量类型赋值给string类型.


#### 3.5.3 数字字面量类型

number字面量
bigint字面量

所有的二进制、八进制、十进制、十六进制都可以作为数字字面量类型。

```ts
const a0: 0b1 = 1
const b0: 0o1 = 1
const c0: 1 = 1
const d0: 0x1 = 1

const a1: 0b1n = 1n
const b1: 0o1n = 1n
const c1: 1n = 1n
const d1: 0x1n = 1n

const e: -10 = -10 // 正负数都可以当数字字面量
const f: -10n = -10n
```

number字面量类型和biginit字面量类型是number类型和bigint的子类型，所以可以进行赋值

```ts
const num: number = a0
const num1: bigint = a1
```

#### 3.5.4 枚举成员字面量类型

联合枚举成员类型也可以将其称作枚举成员字面量类型，因为联合枚举成员类型使用枚举成员字面量形式表示。

```ts
const enum Direction {
    Up,
    Right,
    Down,
    Left
}
const up: Direction.Up = Direction.Up
const right: Direction.Right = Direction.Right
```

### 3.6 单元类型

单元类型也叫单例类型，是指仅包含一个可能值的类型。
ts的单元类型有以下：

1. undefined
2. null
3. unique symbol
4. void
5. 联合枚举成员类型
6. 字面量类型

### 3.7 顶端类型

顶端类型涵盖了类型系统中所有可能的值。
ts中有以下2种顶端类型：

1. any
2. unknown

#### 3.7.1 any

any类型使用any关键字作为标识。在ts中，所有类型都是any的子类型，所以可以将任何类型的值赋值给any，也可以将any类型的值赋值给任意类型。

```ts
let x: any;

x = true; // boolean
x = 'hi'; // string
x = 3.14; // number
x = 99999n; // bigint
x = Symbol(); // Symbol
x = undefined; // undefined
x = null; // null
x = {};
x = [];
x = function () {};


let a: boolean   = x;
let b: string    = x;
let c: number    = x;
let d: bigint    = x;
let e: symbol    = x;
let f: void      = x;
let g: undefined = x;
let h: null      = x;
```

使用any类型来跳过编译器的类型检查

ts中类型注解是可选的，若没有声明类型注解编译器又无法自动推断，那么这个值的默认类型为any类型
如果想避免这种情况，可以启用--noImplicitAny编译选项，当启用时，发生隐式的any类型转换,那么会产生编译错误.


#### 3.7.2 unknown

unknown类型使用unknown关键字作为标识.根据顶端类型的性质，任何其他类型都能够赋值给unknown类型，该行为与any类型是一致的.

```ts
let x: unknown;

x = true; // boolean
x = 'hi'; // string
x = 3.14; // number
x = 99999n; // bigint
x = Symbol(); // Symbol
x = undefined; // undefined
x = null; // null
x = {};
x = [];
x = function () {}
```

unknown类型比any类型安全，unknown类型只可以赋值给any类型和unknown类型

```ts
let x: unknown

// right
let a1: unknown = x
let b1: any = x
// wrong
let a: boolean   = x;
let b: string    = x;
let c: number    = x;
let d: bigint    = x;
let e: symbol    = x;
let f: void      = x;
let g: undefined = x;
let h: null      = x;
```

在程序中使用unknown类型时，我们必须将其细化为某种具体类型，否则将产生编译错误.

```ts
function f1(message: any) {
    return message.length; // 无编译错误
}

f1(undefined);

function f2(message: unknown) {
    return message.length; // wrong : Cannot read properties of undefined (reading 'length') 

    if (typeof message === 'string') {
      return message.length 
    }
}

f2(undefined);
```

  TypeScript中仅有any和unknown两种顶端类型。
  TypeScript中的所有类型都能够赋值给any类型和unknown类型，相当于两者都没有写入的限制。
  any类型能够赋值给任何其他类型，唯独不包括马上要介绍的never类型。
  unknown类型仅能够赋值给any类型和unknown类型。
  在使用unknown类型之前，必须将其细化为某种具体类型，而使用any类型时则没有任何限制。
  unknown类型相当于类型安全的any类型。这也是在有了any类型之后，TypeScript又引入unknown类型的根本原因。

### 3.8 尾端类型

尾端类型是所有类型的子类型，也称作0类型或空类型。
ts中只存在一种尾端类型。即never

#### 3.8.1 never

never类型： 

```ts
let x: never;
function f(): never {
  throw new Error()
}
```

never类型是其他类型的子类型，所以允许赋值给任何类型
never类型不是任何类型的子类型，除never类型自身外，其他类型的值都不能赋值给never类型。any类型也不可以赋值给never类型。

使用场景：

1. never类型可以作为函数的返回值，表示该函数无法返回一个值。没有return语句的函数返回值类型是void，只有函数无法返回一个值的情况函数返回值才是never类型。比如函数抛出异常或者返回体内无限循环永不结束

2. 在“条件类型”中常使用never类型来帮助完成一些类型运算
3. 类型推断，在ts编译器执行类型推断操作时，如果发现无可用的类型那么推断结果为never类型

```ts
function getLength(message: string) {
  if (typeof message === 'string') {
    message // string
  } else {
    message // never
  }
}
```

### 3.9 数组类型

#### 3.9.1 数组类型定义

1. 简便数组类型表示法
2. 泛型数组类型表示法

简便数组类型表示法：
TElement[]
TElement代表数组元素的类型，[]代表数组类型。例：

```ts
const digits: number[] = [0,1,2,3,4,5,6]
```

如果数组中元素为复合类型，例既有number又有string，TElement需要用分组运算符()包裹

```ts
const red: (number | string)[] = ['red', 0, 1]
```

泛型数组类型表示法:泛型数组类型表示法就是使用泛型来表示数组类型,语法：
Array<TElement> Array代表数组类型；“<TElement>”是类型参数的语法

```ts
const digits1: Array<number> = [0,1,2,3]
const red1: Array<number | string> = [0,1,2,3, 'red'] // 数组元素复合类型
```

#### 3.9.2 数组元素类型

在定义了数组类型之后，当访问数组元素时能够获得正确的元素类型信息。示例如下：

```ts
const digits1: Array<number> = [0,1,2,3]
const zero: number = digits1[0]
const zero1 = digits1[0] // zero1 为number
const out: number = digits1[100] // 及时越界还是会得到声明的数组元素类型
```

#### 3.9.3 只读数组

只读数组与常规数组的区别在于，只读数组仅允许程序读取数组元素而不允许修改数组元素。

ts提供以下3种方式定义只读数组：

1. 使用ReadonlyArray<T>内置类型
2. 使用readonly修饰符
3. 使用Readonly<T>工具类型

```ts
const red: ReadonlyArray<number> = [255, 0, 0]
const green: readonly number[] = [0,255,0] // readonly不允许和泛型数组表示法一起使用
const green1: readonly Array<number> = [0,255,0] // wrong

const blue: Readonly<number[]> = [0,0,255] // 类型参数T的值为数组类型“number[]”，而不是数组元素类型number
```

注：在进行赋值操作时，允许将常规数组类型赋值给只读数组类型，但是不允许将只读数组类型赋值给常规数组类型

```ts
const digits: number[] = [0,1,2,3,4,5,6]
const ra: readonly number[] = digits
const a: number[] = arr // wrong
```

### 3.10 元组类型

元组表示有限元素构成的有序列表，js没有元组定义，ts中使用数组表示元组。
在ts中元组是数组的子类型，元组是长度固定的数组，并且数组中每个元素类型都是确定的。

#### 3.10.1 元组的定义

语法如下：
[T0, T1, ...Tn]
该语法中的T0、T1和Tn表示元组中元素的类型，针对元组中每一个位置上的元素都需要定义其数据类型。

```ts
const point: [number, number] = [0, 0]
const score: [string, number] = ['math', 100]
```

赋值时必须保持类型兼容和数量一致

#### 3.10.2 只读元组

只读元组类型是只读数组类型的子类型。定义只读元组有以下两种方式：

1. 使用readonly修饰符
2. 使用Readonly<T>工具类型

```ts
const point: readonly [number, number] = [0, 0]
const score: Readonly<[string, number]> = ['math', 100]
```

注：在给只读元组类型赋值时，允许将常规元组赋值给只读元组类型，不允许将只读元组赋值给常规元组类型。

```ts
const point: [number, number] = [0, 0]

const point1: readonly [number, number] = [0, 0]

const x: readonly [number, number] = point
const y: [number, number] = point1 // wrong
```

#### 3.10.3　访问元组中的元素

在访问元组中指定位置上的元素时，编译器能够推断出相应的元素类型

```ts
const score: [string, number] = ['math', 100]

const a = score[0] // string
const b = score[1] // number
const c: number = score[1]
const d = score[2] // wrong 访问元组中不存在元素编译错误
```

#### 3.10.4 元组类型中的可选元素

定义元组可选元素的语法是在元素类型之后添加一个问号“?”，具体语法如下所示：
[T0?, T1?, ...Tn?]
如果元组中同时存在可选元素和必选元素，那么可选元素必须位于必选元素之后，具体语法如下所示：
[T0, T1?, ..., Tn?]

```ts
let tuple: [boolean, string?, number?] = [true, 'yes', 1];

tuple = [true];
tuple = [true, 'yes'];
tuple = [true, 'yes', 1];
```

#### 3.10.5　元组类型中的剩余元素

语法：[...T[]]
T表示剩余元素的类型。

如果元组类型的定义中含有剩余元素，那么该元组的元素数量是开放的，它可以包含零个或多个指定类型的剩余元素。

```ts
let tuple: [number, ...string[]];

tuple = [0];
tuple = [0, 'a'];
tuple = [0, 'a', 'b'];
tuple = [0, 'a', 'b', 'c'];
```

#### 3.10.6　元组的长度

对于不包含可选元素和剩余元素的元组而言，元组中元素的数量是固定的。
TypeScript编译器能够识别出元组的长度并充分利用该信息来进行类型检查

```ts
function f(point: [number, number]) {
    // 编译器推断出length的类型为数字字面量类型2
    const length = point.length;

    if (length === 3) {       // 编译错误！条件表达式永远为 false
        // ...
    }
}
```

对于包含可选元素的元组，元组长度不固定，编译器能够根据元组可选元素识别出元组所有可能的长度，构造出一个联合类型来表示元组的长度

```ts
let tuple: [boolean, string?, number?] = [true, 'yes', 1];

let len = tuple.length // 1|2|3
len = 1
len = 2
len = 3
len = 4 //wrong: Type '4' is not assignable to type '1 | 2 | 3'.
```

如果元组类型中定义了剩余元素，该元组length属性的类型将放宽为number类型

```ts
let tuple: [number, ...string[]] = [0, 'a'];
let len = tuple.length // number
```

#### 3.10.7　元组类型与数组类型的兼容性

元组类型是数组类型的子类型，只读元组类型是只读数组类型的子类型,允许将元组类型赋值给类型兼容的元组类型和数组类型
元组类型允许赋值给常规数组类型和只读数组类型，但只读元组类型只允许赋值给只读数组类型
由于数组类型是元组类型的父类型，因此不允许将数组类型赋值给元组类型

### 3.11 对象类型

三种基本的对象类型：

  Object类型（首字母为大写字母O）
  object类型（首字母为小写字母o）
  对象类型字面量

#### 3.11.1 Object

这里的Object是指Object类型而不是Object()构造函数。
Object.prototype是一个特殊的对象，是js中的公共原型对象。
Object类型是Object.prototype对象的类型。主要作用是描述js中几乎所有共享（通过原型继承）的属性和方法。

类型兼容性：除了undefined和null外其他类型都可以赋值给Object类型。

```ts
let x: Object
x = true
x = 1
x = 'str'
x = {a:1}
x = [1,2]

x = undefined // 编译错误：Type 'undefined' is not assignable to type 'Object'.
x = null // 编译错误： Type 'null' is not assignable to type 'Object'.
```

Object类型描述了所有对象共享的属性和方法，而JavaScript允许在原始值上直接访问这些方法，因此TypeScript允许将原始值赋值给Object类型。

```ts
let str: Object = 'str'
str.valueOf()
```

js存在自动封箱操作，当在原始值上调用某个方法时，js会对原始值进行封箱操作，并将其转化为对象类型，然后再调用相应的方法。

封箱（boxing）指将原始值包装成一个对象的过程，执行封箱操作后，就能像使用对象一样使用原始数据类型。

```ts
// 自动封箱，将'hi'转换为String对象类型
'hi'.toUpperCase();

// 自动封箱，将3转换为Number对象类型
// 注意：这里使用了两个点符号
3..toString()
```

注：不要将Object类型应用于自定义变量，参数或属性等类型，因为Object类型是Object.prototype对象的类型，是所有对象共享的属性和方法。

#### 3.11.2 object

非原始类型。
在object类型中不允许读取和访问自定义属性，只允许访问对象的共享属性和方法，也就是Object类型中定义的属性和方法。

```ts
let obj: object = {x: 1}
obj.x // 编译错误 Property 'x' does not exist on type 'object'.

obj.toString() // right
```

不允许将原始类型赋值给object类型，这和Object类型不一样，Object类型允许将原始类型赋值给它。
只要非原始类型也就是对象类型才能赋值给object类型

```ts
let nonPrimitive: object;
// wrong
nonPrimitive = 1
nonPrimitive = true
nonPrimitive = 'str'
nonPrimitive = undefined
nonPrimitive = null
nonPrimitive = Symbol()
nonPrimitive = 1n
// 正确
nonPrimitive = {};
nonPrimitive = { x: 0 };
nonPrimitive = [0];
nonPrimitive = new Date();
nonPrimitive = function () {};
```

object类型仅能够赋值给3种类型：

1. 顶端类型any和unknown: 因为任何类型都是顶端类型的子类型
2. Object类型
3. 空对象类型字面量{}

```ts
const nonPrimitive: object = {};

const a: any = nonPrimitive;
const b: unknown = nonPrimitive;

const obj: Object = nonPrimitive

const obj1: {} = nonPrimitive
```

#### 3.11.3 对象类型字面量

对象类型字面量是定义对象类型的方法之一。

```ts
const point: { x: number, y: number} = { x: 0, y: 1}
```

对象类型字面量的类型成员可分为以下五类：

1. 属性签名
2. 调用签名
3. 构造签名
4. 方法签名
5. 索引签名

属性签名：

```ts
let point: {x: number, y: number } = { x:0, y: 1 };

// 可计算属性签名
const a: 'a' = 'a'
let obj: {
  [a]: boolean
}
// 可计算属性名的类型为“unique symbol”类型


let obj1: { x; y} // 允许省略签名类型，该属性类型默认为any
// 等同于
{
  x: any; // noImplicitAny启用时，编译错误
  y: any;
}


// 可选属性
let point1: { x: number, y: number, z?: number}
point = {x:0,y:1}
point = {x:0,y:1, z=0}

// 只允许访问已经定义的必选属性和可选属性
point.x;
point.y;
point.z;

point.t; // wrong


// 只读属性
let point2 : {
  readonly x: number;
  readonly y: number;
}
point2 = { x:0, y:0}
// 只读属性的值在初始化后不允许再被修改
point2.x = 1 // wrong


// 不允许在空对象类型访问任何自定义属性
const point3: {} = { x: 0, y: 0 };
 
point3.x;
//    ~
//    编译错误！属性 'x' 不存在于类型 '{}'

point3.y;
//    ~
//    编译错误！属性 'y' 不存在于类型 '{}'

// 在空对象类型字面量{}上，允许访问对象公共的属性和方法，也就是Object类型上定义的方法和属性。
point3.valueOf()
```

#### 3.11.4 弱类型

弱类型同时满足以下条件的对象类型：

1. 对象类型中至少包含一个属性。
2. 对象类型中所有属性都是可选属性。
3. 对象类型中不包含字符串索引签名、数值索引签名、调用签名和构造签名

```ts
let config: {
  url?: string,
  async?: boolean,
  timeout?: number
}
```

### 3.12 函数类型

#### 3.12.1 常规参数类型

函数形参类型定义：

```ts
function add(x: number, y: number) {
  return x + y
}
// 函数表达式和匿名函数
const f = function(x: number, y: number) {
  return x + y
}


function add(x, y) {
  return x + y // 参数x 和 y没有定义类型，也无法推断类型，那么x和y隐式获得any类型，如果启用了noImplicitAny,那么会产生编译错误，可以指明为any类型
}
```

#### 3.12.2 可选函数参数类型

在调用函数时，编译器会检查传入实际参数的个数与函数定义中形式参数的个数是否相等。如果两者不相等，则会产生编译错误。如果一个参数是可选参数，那么就需要在函数类型定义中明确指定。

```ts
// 可选参数位于必选参数后面
function add(x: number, y?: number, z?: number) {
  return x + (y ?? 0) + (z ?? 0)
}
```

#### 3.12.3 默认参数类型

如果定义了默认参数，并且默认参数在参数列表的末尾，那么该参数将被视为可选参数。例：

```ts
function add(x: number, y = 0) {
  return x + y // y为可选参数，其类型被推断为number
}
add(1) // 1
add(1,3) // 4

function add(x: number = 0, y: number) {} // 此时x不是可选参数，为必选参数

function add(x?: number = 0) {} // 编译错误，同一个参数不能同时出现默认参数和可选参数
```

#### 3.12.4 剩余参数类型

剩余参数类型处理的是多个参数，剩余参数类型应该为数组类型或者元组类型。

```ts
// 数组类型
function f(...args: number[]) {}
f()
f(1)
f(1,2)
f(1,2,3)

// 元组类型
function f(...args: [boolean, number]) {}
// 等同于
function f(args_0: boolean, args_1: number) {}

// 可选元素的元组类型
function f0(...args: [boolean, string?]) {}
// 等同于
function f1(args_0: boolean, args_1?: string) {}

// 带有剩余元素的元组类型
function f0(...args: [boolean, ...string[]]) {}
// 等同于
function f1(args_0: boolean, ...args_1: string[])
```

#### 3.12.5 解构参数类型

解构可以用在函数参数列表中。

```ts
function f0([x,y]) {}
f0([0,0])
function f1({x,y}) {}
f1({x:0,y:0})

// 使用类型注解为解构参数添加类型信息
function f0([x,y]: [number, number]) {}
f0([0,0])
function f1({x,y}: {x: number, y: number}) {}
f1({x:0,y:0})
```

#### 3.12.6　返回值类型

```ts
function add(x: number, y: number): number {
  return x + y
}

function f0():void{}
function f1():void {
  return undefined // right
  return false // wrong
  return 0 // wrong
  return '' // wrong
  return null // strictNullChecks= false void类型返回值可以返回null值
}
```

#### 3.12.7 函数类型字面量

```ts
let f: () => void

let add: (x: number, y: number) => number;

let f: (number) => number;
//      ~~~~~~
//      编译错误 形参必须命名

// 形参和实际参数名不必相同
let f: (x: number) => number;

f = function (y: number): number {
    return y;
}

let bar: () => ; // 编译错误，必须指定函数返回值类型
```

#### 3.12.8 调用签名

函数是一个可调用的对象，可以用对象类型来表示函数类型。
若在对象类型中定义了调用签名类型成员，那么我们称该对象类型为调用签名类型。语法：
{
  (parameterList): Type
}
parameterList表示函数参数形式参数列表类型，Type表示返回值类型，两者都是可选的。

```ts
let add: { (x: number, y: number) => number }
add = function (x: number, y: number): number {
  return x + y
}

// 函数类型字母量完全等同于仅包含一个类型成员且是调用签名类型的对象类型字面量

{
  (parameterList) : Type
}
// 简写为：
(parameterList) => Type
// 函数类型字面量
const abs0: (x: number) => number = Math.abs

// 调用签名
const abs1: { (x: number): number } = Math.abs

abs0(-1) === abs1(-1) // true
```

#### 3.12.9 构造函数类型字面量

构造函数用来创建和初始化对象，比如js内置构造函数Date，const date = new Date()

构造函数类型字面量定义构造函数类型方法之一，语法如下：

```ts
new (parameterList) => Type
```

new关键字，parameterList构造函数形式参数列表类型，Type是返回值类型
JavaScript提供了一个内置的Error构造函数，它接受一个可选的message作为参数并返回新创建的Error对象，示例如下：

```ts
const a = new Error()
const b = new Error('error message')

// 使用构造函数字面量来表示Error构造函数类型
let ErrorConsturctior: new (message?: string) => Error
```

#### 3.12.10 构造签名

构造签名与调用签名类似，若在对象类型中定义了构造签名类型成员，那么称该对象为构造签名类型，语法如下：

```ts
{
  new (parameterList): Type 
}
```

new是运算符关键字，ParameterList表示构造函数形式参数列表类型，Type表示构造函数返回值类型，两者都是可选的。


```ts
let Dog: { new (name: string): Object }
Dog = class {
  private name: string;
  constructor(name: string) {
    this.name = name
  }
}
let dog = new Dog('huahua')
```

构造函数字面量完全等同于一个仅包含一个构造函数签名类型的成员的对象类型字面量。

```ts
{
  new (parameterList): Type
}
// 简写为：
new (parameterList) => Type
```

#### 3.12.11 调用签名和构造签名

有一些函数被设计为既可以作为普通函数使用，同时又可以作为构造函数来使用

```ts
const a: number = Number(1)
const b: Number = new Number(1)
```

在对象类型中同时定义调用签名和构造签名，既可以被调用，又可以作为构造函数：

```ts
{
  new (x: number): Number; // 构造签名
  (x: number): number // 调用签名
}

declare const F: {
  new (x: number): Number; // 构造签名
  (x: number): number // 调用签名
}

const a: number = F(1) // 普通函数调用
const b: Number = new F(1) // 构造函数调用

```

#### 3.12.12 重载函数

重载函数是指一个函数同时拥有多个同类的函数签名，比如一个函数拥有2个及以上调用签名，一个构造函数同时拥有2个及以上构造签名。
函数重载定义组成：

1. 一条或多条函数重载语句
2. 一条函数实现语句

函数重载：不带有函数体的函数声明语句。只存在于代码编译阶段，生成js代码后会被删除

```ts
function add(x: number, y: number): number; // 函数重载
```

函数重载允许存在一个或多个，只有多于1个的才有意义。函数重载不允许使用默认参数，函数重载位于函数体实现之前，必须和函数实现中的函数名一致。例如：

```ts
function add(x: number, y: number): number;
function add(x: any[], y: any[]): any[];
function add(x: number | any[], y: number | any[]): any {
  // 实现代码
}
```

函数重载和函数重载，函数重载和函数体实现语句中间不允许出现别的语句，否则编译错误。

函数实现：包含了函数体代码，存在编译前和编译后，只允许有一个函数实现且位于所有函数重载之后。

函数实现必须兼容每个函数重载中的函数签名。函数实现的函数签名类型必须能够赋值给函数重载的函数签名类型。

#### 3.12.13 函数中this值的类型

this是js中关键字，表示调用函数的对象或实例对象。
默认情况，编译器会将函数中的this值设为any类型，允许程序在this上执行任意的操作。