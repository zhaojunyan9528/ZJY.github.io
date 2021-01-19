---
title: es6-es6知识点
tags:
  - 前端
categories:
  - - 笔记
    - es6
date: 2021-01-19 17:39:20
---

### 1. 块作用域-let {}

### 2. 恒量-const

### 3. 解构数组 
let arr = [1,2,3] console.log(...arr)

### 4. 解构对象 
let obj = {a:a,b:b,c:c} = {a:1,b:2,c:3} console.log(a,b,c)// 1 2 3

### 5. 模板字符串： 
let day = 'sunnday'  `today is ${day}`

### 6. 带标签的模板字符串：

```javascript
    let dessert = 'bread',drink='milk'
    let breakfast = kitcten`today's breakfast is ${dessert} and ${drink}`
    function kitcen(stings,...values) {
        console.log(strings); //['today's breakfast is','and']
        console.log(values); // ['bread','mill']
    }
```

### 7. startsWith() endsWith() 判断是否包含其他字符串 返回布尔值

### 8. 默认参数：

```javascript
    function breakfast(dessert = 'bread', drink='milk'){
        return dessert + drink
    }
```

### 9. ...展开操作符

```javascript
    let arr = ['1','2']
    console.log(arr) // ['1','2']
    console.log(...arr) // 1  2
```

### 10. ...剩余操作符

```javascript
    fuction (dessert,drink,...foods){}  breakfast('bread','milk','orange','apple')
```

### 11. 函数的名字

```javascript
    function test() {} console.log(test.name)
```

### 12. 箭头函数

### 13. 对比两个值是否相等：

```javascript
    +0 == -0 // true
    +0 === -0 // true
    NaN == NaN // false
    Object.is(NaN,NaN) //true
    Object.is(+0,-0) //false
```

### 14. Object.assign

### 15. 设置对象的prototype:

```javascript
    let breakfast = {
        getDrink() {
            return 'tea'
        }
    }
    let dinner = {
        getDrink() {
            return 'beer'
        }
    }
    let sunday = Object.create(breakfast)
    console.log(sunday.getDrink()); //tea
    console.log(Object.getPrototypeOf(sunday) === breakfast); //true
    Object.setPrototypeOf(sunday,dinner)
    console.log(sunday.getDrink()); // beer
    console.log(Object.getPrototypeOf(sunday) === dinner); //true

```

### 16.__proto__

```javascript
    let sunday = {
        __proto__: breakfast
    }
    console.log(sunday.breakfast); // tea
    console.log(Object.getPrototypeOf(sunday) === breakfast); //true
    sunday.__proto__ = dinner
    console.log(sunday.getDrink()); // beer
    console.log(Object.getPrototypeOf(sunday) === dinner); //true
```

### 17. super

```javascript
    let sunday = {
        __proto__: breakfast,
        getDrink() {
            return super.getDrink() + 'milk'
        }
    }
    console.log(sunday.getDrink()) // teamilk
```

### 18. interator迭代器

    每次返回一个对象{value:xxx, done:true/false}

### 19. generators生成器

```javascript
    function* foods(){
        yeild 'apple'
        yeild 'orange'
    }
    let test = foods();
    console.log(test.next()) // {value:'apple',done:false}
    console.log(test.next()) // {value:'apple',done:false}
    console.log(test.next()) // {value:undefined,done:true}
```

### 20. class 类  

```javascript
    class Chef {
        constructor(food){
            this.food = food
        }
        cook() {
            console.log(this.food);
        }
    }
    let test = new Chef('apple')
    test.cook() //apple
```

### 21. get set

```javascript
    class Chef {
        constructor(food){
            this.food = food
            this.dish = []
        }
        get menu(){
            return this.menu
        }
        set menu(dish) {
            this.dish.push(dish)
        }
        cook() {
            console.log(this.food);
        }
    }
    let test = new Chef()
    test.menu = 'apple'
    test.menu = 'banana'
    console.log(test.menu) // ['apple','bannana']
```

### 22. static 静态方法不需要实例化就可以使用的方法

```javascript
    static cook(foods){
        console.log(this.foods)
    }
    Chef.cook('apple')
```

### 23. extends

```javascript
    class Person {
        constructor(name,birthday) {
            this.name = name
            this.birthday = birthday
        }
        intro () {
            return `${this.name},${this.birthday}`
        }
    }

    class Chef extends Person {
        constructor(name,birthday) {
            super(name,birthday)
        }
    }
    let test = new Chef('zjy','1995-2-8')
    console.log(test.intro()); //zjy,1995-2-8
```

### 24. Set 集合(不能有重复值)

```javascript
    let desserts = new Set()
    console.log(desserts); // {}
    desserts.add('apple')
    desserts.add('apple')
    console.log(desserts); //{'apple'}
    console.log(desserts.size) //1
    console.log(desserts.has('apple')) //true
    console.log(desserts.delete('apple'))l
    desserts.forEach(dessert=>{
        console.log(dessert)
    })
    desserts.clear()//清空
```

### 25. Map

```javascript
    let food = new Map()
    let friut = {},
        cook = function() {},
        dessert = 'sweeatie'

    food.set('friut','lemon')
    food.set('cook','fork')
    food.set('dessert','bread')

    console.log(food); //{"friut" => "lemon", "cook" => "fork", "dessert" => "bread"}
    console.log(food.size); //3
    console.log(food.get(fruit));
    food.delete(dessert)
    food.has(dessert)
    food.forEach((value,key)=>{})
    food.clear()
```

### 26. module

```javascript
    export {xxx}  import { xxx}
    export default 
    export { xxx as default }
    export default  //默认导出引用时不需{}
```