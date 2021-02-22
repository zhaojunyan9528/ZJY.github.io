---
title: typescript
tags:
  - 前端
categories:
  - - 笔记
    - TypeScript
date: 2021-02-21 14:54:14
---

## TS（typescript）是什么？

TS是以JavaScript为基础构建的语言，是JavaScript的一个超集
TS扩展了JavaScript，并添加了类型。可以在任何支持JavaScript的平台执行。
TS不能被js解释器直接执行，需要将ts编译成js

## TS增加了什么？

类型
支持ES的新特性
添加ES不具有的新特性
丰富的配置选项（编译为ES3，ES6等）
强大的开发工具

## TS安装

1.安装node
2.安装npm
3.npm install -g typescript
4.tsc -v  查看tsc安装版本

## TS编译  

tsc ts.ts
编译成功生成同名js文件

## 类型声明： 

```js
let a:number = 10;或者let a = 10;
function sum(a:number, b:number):number{
    return a + b;
}
```

## 联合类型

联合类型（Union Types）可以通过管道(|)将变量设置多种类型，赋值时可以根据设置的类型来赋值。

注意：只能赋值指定的类型，如果赋值其它类型就会报错

```js
let d: 'male' | 'female';
d = 'male';
d = 'female';

let e: boolean | string;
e = false;
e = '123';
```

## TypeScript基础类型

| 数据类型 | 关键字 | 描述  |
| ------ | ----- | ----- |
| 任意类型 | any     | 声明为 any 的变量可以赋予任意类型的值|
| 数字类型 | number  |  双精度 64 位浮点值。它可以用来表示整数和分数。<br>let binaryLiteral: number = 0b1010; // 二进制 <br>let octalLiteral: number = 0o744;    // 八进制 <br>let decLiteral: number = 6;    // 十进制 <br>let hexLiteral: number = 0xf00d;    // 十六进制 |
| 字符串类型 | string  | 一个字符系列，使用单引号（'）或双引号（"）来表示字符串类型。反引号（``）来定义多行文本和内嵌表达式。<br>let name: string = "Runoob";<br>let years: number = 5;<br>let words: string = `您好，今年是 ${ name } 发布 ${ years + 1} 周年`; |
| 布尔类型 | boolean | 表示逻辑值：true 和 false。<br>let flag: boolean = true;|
| 数组类型 | 无     | 声明变量为数组。<br>// 在元素类型后面加上[]<br>let arr: number[] = [1, 2];<br>// 或者使用数组泛型<br>let arr: Array<number> = [1, 2];|
| 元组     | 无     | 元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。<br>let x: [string, number];<br>x = ['Runoob', 1];    // 运行正常<br>x = [1, 'Runoob'];    // 报错<br>console.log(x[0]);    // 输出 Runoob |
| 枚举 | enum | 用于定义数值集合 <br>enum Color {Red, Green, Blue};<br>let c: Color = Color.Blue;<br>console.log(c);    // 输出 2
| void | void | 用于标识方法返回值的类型，表示该方法没有返回值<br>function hello(): void {<br>alert("Hello Runoob");<br>或者return; /return null; /return undefined} |
| null | null | 表示对象值缺失 |
| undefined | undefined | 用于初始化变量为一个未定义的值|
| never | never | never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值

## any

```js
let a;
a = 123;
a = 'fs';
a = true;

let s:string;
s = a; //正常

let c: unknown; //未知类型的值
c = 123;
c = 'fs';
c = true;
c = 'str';

s = c; //错误：Type 'unknown' is not assignable to type 'string'

// unknown实际上是一个类型安全的any
// unknown类型的变量不可以直接赋值给其他变量，需要类型检测，any类型的可以直接赋值。
if(typeof c === 'string'){
    s = c; //正常
}
// 或者：
s = h as string; //类型断言
s = <string>e; //类型断言
```

## 类型断言(Type Assertion)

类型断言可以用来手动指定一个值的类型，即允许变量从一种值的类型转换为另一种类型。
语法：

+ <类型>值
+ 值 as 类型

## never 不可能出现的值

```js
//表示不会有返回值的
function test1(): never{
    throw new Error('error')
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}

let n:never;
let m: string;
// n = 1; // 运行错误，数字类型不能转为 never 类型
// <!-- 运行正确，never 类型可以赋值给 never类型 -->
n = (()=>{ throw new Error('error')})();

m = '1';
// <!-- 运行正确，never 类型可以赋值给 字符串类型 -->
m = (()=>{ throw new Error('error')})();
```

## 对象

{}可以指定对象中有哪些属性

```js
let obj: {name: string,age?:number};
obj = {name: 'zjy'};
```

age?:number,?表示可选

多个属性可选：[propName: string]:any

```js
let obj: {name: string,[propName:string]:any};
obj = {name:'zjy',age:13,sex:1}
```

函数结构的类型声明：
语法：(属性：类型,...)=>返回值

```js
let fun :(a:number,b:number)=>number;
fun = function(n1:number,n2:number):number{
    return n1 + n2;
}
```

## &:表示同时满足2个条件

```js
let obj2: {name:string} & {age:number};
    obj2 = { name: 'zjy', age:18}
```

## 类型的别名

```js
// 类型的别名
type typename = 1 | 2 | 3;
let k: typename;
k = 3
```

## 自动编译选项：tsc xxx.ts -w

## 添加tsconfig.json文件可编译目录下所有文件

tsc -init 生成tsconfig.json文件

## tsconfig.json编译选项

+ include: [] 用于指定哪些文件需要被编译
  + **: 任意目录
  + *: 任意文件
+ exclude :[] 不需要被编译文件的目录
  + "exclude"默认情况下会排除node_modules，bower_components，jspm_packages和`<outDir>`目录

+ extends： 可以利用extends属性从另一个配置文件里继承配置
+ files: [] 指定一个包含相对或绝对文件路径的列表
  + 使用"include"引入的文件可以使用"exclude"属性过滤。 然而，通过 "files"属性明确指定的文件却总是会被包含在内，不管"exclude"如何设置
+ compilerOptions:编译器选项配置
  + target：指定ECMAScript目标版本，默认ES3，"ES5"， "ES6"/ "ES2015"， "ES2016"， "ES2017"或 "ESNext"。
  + module: 指定生成哪个模块系统代码： "None"， "CommonJS"， "AMD"， "System"， "UMD"， "ES6"或 "ES2015"。
  + lib:[]  编译过程中需要引入的库文件的列表,例如ES5，ES6 ，DOM等
  + outDir: 用来指定编译后文件所在目录
  + outFile: 将输出文件合并为一个文件,只有 "AMD"和 "System"能和 --outFile一起使用。
  + allowJs: 默认是false，允许编译javascript文件。
  + checkJs: 默认是false，是否检测js语法，与allowJs配合使用
  + removeComments: 默认是false，删除所有注释，除了以 /!*开头的版权信息。
  + noEmit: 默认是false,不输出编译文件
  + noEmitOnError: 默认是false 报错时不生成输出文件
  + alwaysStrict: 默认是false 以严格模式解析并为每个源文件生成 "use strict"语句,当文件使用模块化时自动采用严格模式，不会生成文件头部'use strict'
  + noImplicitAny: 默认是false 在表达式和声明上有隐含的 any类型时报错。
    + 例如：function add(a,b){
      return a + b;
      }
  + noImplicitThis: 默认是false 当 this表达式的值为 any类型的时候，生成一个错误
  + strictNullChecks: 默认是false 严格检查空值
  + strict: 默认是false 启用所有严格类型检查选项。启用 --strict相当于启用 --noImplicitAny, --noImplicitThis, --alwaysStrict， --strictNullChecks和 --strictFunctionTypes和--strictPropertyInitialization
  
tsconfig.json文件

```js
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es6",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "es6",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    // "lib": [],                             /* Specify library files to be included in the compilation. */
    "allowJs": true,                       /* Allow javascript files to be compiled. */
    "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "dist/app.js",                       /* Concatenate and emit output to single file. */
    "outDir": "dist/",                        /* Redirect output structure to the directory. */
    // "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    "removeComments": true,                /* Do not emit comments to output. */
    "noEmit": false,                        /* Do not emit outputs. */
    "noEmitOnError": false,
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    "noImplicitAny": true,                 /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */
    // "noUncheckedIndexedAccess": true,      /* Include 'undefined' in index signature results */

    /* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "skipLibCheck": true,                     /* Skip type checking of declaration files. */
    "forceConsistentCasingInFileNames": true  /* Disallow inconsistently-cased references to the same file. */
  },
  "include": [
      "src/**/*"
  ],
}
```