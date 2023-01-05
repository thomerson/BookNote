## 函数

* 定义式

```javascript
function functionName(parameters) {
  //执行的代码
}

```

* 变量式

实际上是一个 **匿名函数**


```javascript
var fun = function () {
    //执行代码
};

```

## arguments

```javascript
类数组对象，包含着传入函数中的所有参数

function sum1(num1, num2, num3) {
    var temp = 0;
    if (arguments.length == 3) {
        //***********具体实现其逻辑*********************
        for (var i = 0; i < arguments.length; i++) {
            temp += arguments[i];
        }
    }
    else {
        //***********具体实现其逻辑*********************
        for (var i = 0; i < arguments.length; i++) {
            temp += arguments[i];
        }
    }
    return temp;
}
alert("1+2=" + sum1(1, 2) + "  1+2+3=" + sum1(1, 2, 3));
```

## 构造函数

 js构造函数特殊的地方：返回值

1. 没有手动添加返回值，默认返回 this 。

2. 手动添加一个基本数据类型的返回值，最终还是返回 this

```javascript
function Person2() {
  this.age = 28;
  return 50;
}

var p2 = new Person2();
console.log(p2.age);   // 28
```

3. 手动添加一个复杂数据类型(对象)的返回值，最终返回该对象

```javascript
function Person3() {
  this.height = '180';
  return ['a', 'b', 'c'];
}

var p3 = new Person3();
console.log(p3.height);  // undefined
console.log(p3.length);  // 3
console.log(p3[0]);      // 'a'
```


## 函数自执行

```javascript
()()
(function () {
    var x = "Hello!!";      // 我将调用自己
})();
!function() {}() // ！运算 false 
+function() {}() // +运算 NaN

```

