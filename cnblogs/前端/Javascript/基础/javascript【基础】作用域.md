## 作用域

```javascript
if (true) {
var str = "李四";
}
alert(str);   //李四  !!js中没有块级作用域
 
```

js中只有**函数作用域**和**全局作用域**

 

## 作用域链

先从自己的作用域取变量，没取到然后去父作用域中取。

```javascript
var num1 = 10;
function fun1() {
  alert(num1);
}
function fun2(fn) {
  var num1 = 12;
  fn();
}
fun2(fun1);  //10
```

 

### 严格模式 "use stinct"

严格模式下**不能使用未声明的变量**。

优点

* 消除代码运行的一些不安全之处，保证代码运行的安全；

* 提高编译器效率，增加运行速度；

* 严格模式下禁止this关键字指向全局对象。

```javascript
function f(){
    return !this;
} 
// 返回false，因为"this"指向全局对象，"!this"就是false

function f(){ 
    "use strict";
    return !this;
} 
// 返回true，因为严格模式下，this的值为undefined，所以"!this"为true。
```

## var和let的区别

* var声明的变量会挂载在window上，let和const声明的变量不会

```javascript
var a = 100;
console.log(a,window.a);    // 100 100

let b = 10;
console.log(b,window.b);    // 10 undefined

const c = 1;
console.log(c,window.c);    // 1 undefined
```

* var声明变量存在变量提升，let和const不存在变量提升

```javascript
console.log(a); // undefined  ===>  a已声明还没赋值，默认得到undefined值
var a = 100;
console.log(b); // 报错：b is not defined  ===> 找不到b这个变量
let b = 10;
console.log(c); // 报错：c is not defined  ===> 找不到c这个变量
const c = 10;
```

let和const声明形成块作用域
```javascript
if(1){
    var a = 100;
    let b = 10;
}

console.log(a); // 100
console.log(b)  // 报错：b is not defined  ===> 找不到b这个变量

if(1){

    var a = 100;
        
    const c = 1;
}
 console.log(a); // 100
 console.log(c)  // 报错：c is not defined  ===> 找不到c这个变量
``` 

* 同一作用域下let和const不能声明同名变量，而var可以

```javascript

var a = 100;
console.log(a); // 100

var a = 10;
console.log(a); // 10
let a = 100;
let a = 10;

//  控制台报错：Identifier 'a' has already been declared  ===> 标识符a已经被声明了。
// 暂存死区
var a = 100;

if(1){
    a = 10;
    //在当前块作用域中存在a使用let/const声明的情况下，给a赋值10时，只会在当前作用域找变量a，
    // 而这时，还未到声明时候，所以控制台Error:a is not defined
    let a = 1;
}
// const
/*
* 　　1、一旦声明必须赋值,不能使用null占位。
*
* 　　2、声明后不能再修改
*
* 　　3、如果声明的是复合类型数据，可以修改其属性
*
* */

const a = 100; 

const list = [];
list[0] = 10;
console.log(list);　　// [10]

const obj = {a:100};
obj.name = 'apple';
obj.a = 10000;
console.log(obj);　　// {a:10000,name:'apple'}

```
