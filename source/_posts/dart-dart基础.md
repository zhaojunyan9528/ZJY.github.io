---
title: dart-dart基础
tags:
  - 前端
categories:
  - - 笔记
    - dart
date: 2021-01-19 17:17:43
---

### 1.安装dart sdk
    
https://dart.dev/get-dart
终端输入dart --version 测试是否安装成功

### 2.配置vscode支持dart语法提示：

1.安装插件dart
2.安装插件code runner  可以运行文件

### 3.vscode运行dart文件中文乱码：

1.在settings.json添加："code-runner.runInTerminal": true,
2.重启vscode

### 4.入口方法
    
main(){};
void main(){};//没有返回值

### 5.注释

//注释
///注释
/*注释*/

### 6.声明变量：

1.通过var声明： var str = "aaa"; //dart强类型校验，可以不预先声明变量类型，自行判断类型
2.通过类型声明  String str = "aaa"

变量命名规则：
        1.必须由数字/字母/下划线和$组成
        2.不能以数字开头
        3.不能是保留字和关键字
        4.区分大小写

### 7.常量： const 和 final

1.const 和finnal 赋值都不可以修改
2.区别： final可以开始不赋值，只能赋值一次，final不仅有const的编译时常量的特性，最重要的是它是编译时赋值
        例如：
        final date = new DateTime.now(); //正常 编译时赋值，只能赋值一次
        const date = new DateTime.now(); //报错 定义时赋值

### 8.dart 数据类型：

常用数据类型：

        Numbers(数值)：int double
        Strings(字符串)： String
        Booleans(布尔值): bool
        List(数组): 列表对象
        Maps(字典)： Map是一个键值对相关的对象

1.字符串

```javascript
String str = "falds"
String str = """flasjd
flad
d;fja""" //三个单引号或者三个双引号字符串可换行
// <!-- 字符串拼接： -->
    print("$str $str");
    print(str +" "+ str);
```

2.数值类型：

```javascript
int a = 12; //只可以是整型
double = 12.23  //既可以是整型也可以是浮点型
```
        
3.布尔类型：

```javascript
bool flag = true/false ; //布尔类型的值只能是true/false
条件判断不会对变量进行类型转换： 123 != '123'
```
        
4.数组类型

```javascript
var arr1 = ['a','b','c']
var arr2 = new List();
arr2.add("a")
```

3.指定数组元素类型

```javascript
var arr3 = new List<String>();
arr3.add("a");
arr3.add(1); //错误
```

5.Maps

```javascript
var person = {
    "name":"test"
}
```

访问： person["name"]

```javascript
var p = new Map();
p["name"] = "test"
```

6.is关键词判断数据类型：

```javascript
 var str = 'ad';
 print(str is String)
```

### 9.运算符

+ 算术运算符
    1. +
    2. -
    3. *
    4. /
    5. % 取余
    6. ~/ 取整

+ 关系运算符：
    1. ==
    2. !=
    3. <
    4. `>`
    5. <=
    6.` >=`

+ 逻辑运算符：
    !
    &&
    ||

+ 赋值运算符：
    `=`
    `??=`  // int b; b??=23 如果b等于空的话，把23赋值给b

+ 复合赋值运算符：
    +=
    -=
    *-
    /=
    ~/=

+ 条件表达式：
    if... else
    if... else if
    swtich... case

    三目运算符 ? :
    ??运算符 b= a ?? 10 //a不为空时b等于a，a为空时b等于10

    b=a++ 赋值运算中： ++写在后面 先赋值后运算 ++a 写在前面先运算后赋值

### 10.类型转换：
+ int.parse
+ double.parge
+ toString
+ str.isEmpty
+ num.isNaN


### 11.List属性：

+ length
+ isEmpty
+ isNotEmpty
+ reversed 翻转列表 [1,2,3].reversed //(3,2,1) [1,2,3].reversed.toList() //[3,2,1]

### 12.List方法：

+ add 数组添加 add(1)
+ addAll([1,2]) //拼接数组
+ indexOf 返回索引值
+ remove 移除值，参数为值
+ removeAt 参数为索引值
+ fillRange int start,int end,修改后的值
+ insert(int index,value)指定位置插入值
+ insertAll(index,interable)  insertAll(1,[1,2]) 
+ join(分割符) 以分隔符拼接转换成字符串
+ split(分隔符) 将string以分隔符切割成数组
+ toList() 将其他类型对象转换为数组


### 13.Set主要功能是数组去重

set是不能重复且没有顺序的对象

### 14.Maps 映射

+ key   var person={"a":1,"b":2} person.keys.toList()
+ value
+ isEmpty
+ isNotEmpty
+ addAll() 添加多个
+ remove(key)
+ containsValue(value) 是否存在某个值

### 15.循环：
```javascript
forEach
map  用于修改数据 var newList = list.map((value){ return value*2})
where  var newList = list.where((value){ return value > 5}) 返回符合条件的数组
any  var newList = list.any((value){ return value > 5}) 有一个满足条件返回true
some ar newList = list.some((value){ return value > 5}) 满足所有条件返回true
```
