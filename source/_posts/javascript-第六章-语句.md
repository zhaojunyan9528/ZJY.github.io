---
title: javascript-第六章-语句
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-01-13 11:54:37
---

## 第六章-语句

### 1. 表达式语句

递增（++）和递减（--）运算符，delete运算符，函数调用，被作为语句使用，因为都改变了值，而不仅仅是表达式的一部分。

### 2. 复合语句

复合语句就是用花括号括起来，将几个语句联合起来形成语句块。js语句可以包含别的语句，这样的语句叫复合语句。
正式的javascript语法规定每个复合语句可以包含一个子语块。那么使用语句块可以将任意数量语句放在子语块中。
javascript解释器执行复合语句时，按照编写顺序执行语句。通常javascript语句会执行完所有语句，但复合语句中若含有break语句、continue语句、return或throw语句，或它引发了错误等会被终止。

### 3. if语句

条件控制语句

```javascript
if(express) //null、undefined、0、“”、NaN值为false
    statement
```

### 4. else if语句

用来执行多个条件语句，实际上是if/else嵌套

### 5. switch语句

用于重复检测同一个变量的值

```javascript
switch(express){
    statement
}
switch(n){
    case 1:
        statement
        break;
    case 2:
        statement
        break;
    default:
        break;
}
```

在函数调用中可以用return替换break语句.
case使用===等同运算符匹配的，如果没有匹配值就执行default:，如果没有default：就跳过主体，default:语句可以放置任何位置。

### 6. while语句

```javascript
while(express)
    statement
    //express表达式值为true，执行statement循环体
```

### 7. do/while语句

```javascript
do
    statement
while(express);
```

循环体至少会被执行一次。用分号结尾.

### 8. for语句

```javacript
for(initalize; test ; increment) //初始化、检测、更新
    statement
```

### 9. for/in语句

```javscript
for(variable in object)
    statement
```

variable是一个变量名，声明一个变量的var语句或者对象的一个属性或者数组的一个元素。

object是一个对象名或者计算结果为对象的表达式。for/in循环主体对object对象对每个属性执行一次，循环体执行之前，对象的属性名会被作为字符串赋值给variable.在循环体内部可以通过object[varible]访问属性的值	。variable可以是任意表达式。

for/in循环体并没有指定将对象属性赋给循环变量的顺序。for/in会遍历对象所有可能属性。对象属性被标记为可读的、永久的（不可删除的）、不可枚举的不可被遍历出来。

### 10. 标签语句

identifier：statement，标签语句：标识符加冒号
标识符identifier可以是合法的js标识符，不能是保留字，标识符不同于变量名和函数名，所以不用担心重名。给语句添加标识符，起一个名字可以在任意地方引用。

### 11. break语句

break语句会使包含在最内层的循环体立即结束或退出switch语句。语句很简单：
    break;
当break后面跟标签时，它会跳到标签语句结尾或者终止这个语句。break labelname；

### 12. continue语句

+ continue语句退出当前循环，执行下一次循环，continue;
+ continue语句可以和标签一起使用在ecmascript v3和js1.2中，continue labelname;
+ continue语句只可以出现在while、for、do/while、for/in循环语句体中。
执行continue语句时，当前迭代就会终止，开始下一次迭代，这对不同循环是不同的：

    + while循环中，会再次检测循环条件，如果是true，继续下一次迭代
    + do/while循环中，会跳到循环的底部，开始下一次迭代前首先判断循环条件
    + for循环中，会先更新循环变量值，再判断循环条件是否继续下一次迭代
    + for/in循环中，以下一个循环属性赋值给变量开始新的迭代

### 13. var语句

var语句允许你明确的声明一个或多个变量。如果声明多个变量，用逗号隔开，并可选的初始化声明的变量值。

var语句通过在封闭函数的调用对象中创建同名的属性来定义变量，如果不在函数内，就在全局对象中创建同名属性来定义变量。

由var语句创建的特性不能被delete运算符删除。

### 14. function语句

function语句定义了js函数：

```javascript
function funcname(args){
    statement
}
```

函数体内语句定义时不会被执行，在使用函数调用（）运算符调用时才执行编译。
函数定义在解析时发生而不是在运行时发生。
函数定义通常出现在代码顶层，它们也可以嵌套在其他函数定义中，但是只能嵌套在函数顶层定义中，也就是说函数定义不能出现在if语句，while循环或其他语句中。

```javascript
alert(f(4)); //16，可以在定义f()前调用它
var f = 0; //该语句重写属性f
function f(x){return x*x}
alert(f); //0，函数f已经被变量f覆盖
```

### 15. return语句

运算符()调用函数是一个表达式，表达式的值通过return语句返回。return只能出现在函数体内，出现在其他地方会造成语法错误。return expression;
在执行return语句时，会先计算表达式的值，然后返回它的值作为函数的值

### 16. throw语句

所谓异常（exception）就是一个信号，说明发生了某种异常状况或错误。
抛出（throw）一个异常，就是用信号通知发生了错误或异常状况。
捕获（catch）一个异常就是处理它，即采取必要或适当的行为从异常恢复。在js中，当发生了运行时错误或用throw语句就会抛出异常。
throw expression;expression可以是任意类型，通常是Error类或子类的实例。
在抛出异常时，javascript解释器会立即停止正常的程序执行，跳到距离最近的异常处理器，即catch语句中。
如果抛出的异常中没有catch语句，即会检测次高级封闭代码块，查看是否有异常处理器，直到没有找到为止。

### 17. try/catch/finally语句

try/catch/finally是javascript的异常处理机制。
try语句定义异常需要被处理的代码块。
catch发生异常时调用的语句块。
finally存放清除代码，无论try发生什么，都会被执行。
catch和finally都是可选的。但是try块至少跟一个catch或finally块。

通常情况下,控制流到达try块的尾部,然后开始执行 finally块,以便进行清除操作。如果由于 return语句、 continue句或 break语句使控制流离开了try块,那么在控制流转移到新目的地前, finally块就会被执行。

如果异常发生在try块中,而且存在一个相关的 catch块处理异常,控制流首先将转移到 catch块,然后再转移到fina1l块。如果没有处理异常的局部 catch块,控制流首先将转移到 finally块,然后向上传播到最近的能够处理异常的 catch从句。

如果 finally块自身用 return语句、 continue语句、 break语句或 throw语句转移了控制流,或者调用了抛出异常的方法改变了控制流,那么等待的控制流转移将被舍弃,并进行新的转移。例如,如果 finally从句抛出了一个异常,那么该异常将代替处于抛出过程中的异常。如果 finally从句运行到了 return语句,那么即使已经抛出了一个异常,而且该异常还没有被处理,该方法也会正常返回。

### 18. with语句

with语句用于暂时修改作用域链。with (object) statement
这一语句能够有效的将object添加到作用域链的顶部，然后执行statement语句，再把作用域链恢复到初始状态。
在实际应用中,使用with语句可以减少大量的输入。例如,在客户端的 JavaScript中,深度嵌套的对象层次很常用。例如,可以输入如下的表达式来访问一个HTML表单的元素:

 frames[1].document.forms[0].address.value

如果需要多次访问这个表单,可以使用with语句将这个表单添加到作用域链中:

```javascript
with(frames [1].document.forms[0]){
    //此处直接访问表单元素
    name.value = ‘’;
    address.value = ‘’;
    email.value = ‘’;
}
```

虽然有时使用with语句比较方便,但是人们反对使用它。使用了with语句的javaScript代码很难优化,因为它的运行速度比不使用with语句的等价代码要慢得多。而且,在with语句中的函数定义和变量初始化可能会产生令人惊讶的、相抵触的行为。因此,我们建议避免使用with语句。

注意,还有其他的、极为合理的方法可以用来节省输入。例如,上面使用with语句可重写为:

```javascript
var form= frames[1]. document. forms[0]
form.name. value="
form. address. value ="" form. email. value =""
```

### 19. 空语句

javascript中最后一个合法语句是空语句: ;

执行空语句显然不会产生任何作用,也不会执行任何动作。你可能认为使用这样一个语句是毫无意义的,但实践证明,当你想创建一个具有空主体的循环时,空语句是有用的。例如:

```javascript
//初始化数组a
for(i=0;i< a. length; a[i++] =0) ;
```

注意,在for循环、 while循环或者if语句的右括号后加分号会产生bug,这些bug很难被检测出来。例如,下面的代码产生的结果并不是作者想要的:

```javascript
if((a==0) || (b==0));/这行什么都不做
o =null; //这行总会被执行
```

当你打算使用空语句时,最好在代码中使用注释,以清楚地说明是有目的地这样做。例如:

```javascript
for(i=0;i<a. length;a[i++]=0)/*空函数体*/ ;
```

### 20.javascript语句小结

| 语句 | 语法 | 用途 |
| :---: | :---: | :---: |
| break | break; <br> break labelname; | 退出最内层循环或者退出switch语句，又或者退出label指定的语句 |
| case | case expression | 在switch语句中标记一个语句 |
| continue | continue;<br>  continue labelname; | 重新开始新的循环或开始指定的label循环 |
| default | default: | 在switch语句中指定默认语句 |
| do/while | do statement <br> while(expression) | while循环另一种形式 |
| 空语句 | ; | 什么都不做 |
| for | for(init;test;increment) statement | 循环 |
| for/in | for(variable in object) statement | 遍历一个对象的属性 |
| function | function funname(expression){statement} | 函数声明 |
| if/else | if(expression){statement} <br> else{} | 条件控制 |
| label | identifier: statement | 给语句块指定一个标识符 |
| return | return expression | 返回函数值 |
| switch | switch(expression){statement} | 由case或default语句标记的多分支语句 |
| throw | throw exception | 抛出一个异常 |
| try | try{statement} <br> catch(e){statement}<br>  finally{statement} | 捕捉一个异常 |
| var | var name1 = value | 声明或初始化变量 |
| while | while(expression){statement} | 循环 |
| with | with(object) statement | 扩展当前作用域链（不支持使用） |

