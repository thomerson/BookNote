## ES6

```ES2015```

## ES6新数据类型

* Symbol

    表示**独一无二的值**，最大的用法是用来定义对象的唯一属性名

    * ```Symbol.for()```

    单例模式，首先会在全局搜索被登记的 Symbol 中是否有该字符串参数作为名称的 Symbol 值，如果有即返回该 Symbol 值，若没有则新建并返回一个以该字符串参数为名称的 Symbol 值，并登记在全局环境中供搜索

    * ```Symbol.keyFor()```

    返回一个已登记的 Symbol 类型值的 key ，用来检测该字符串参数作为名称的 Symbol 值是否已被登记

## ES6新语法

1. 定义变量 let 代替var

2. 定义常量const


3. 字符串连接

    <br>模板引擎

    ```javascript
    var str =    "帅";//注意这里是正常双引号
    var str2 = `你们从我脸上看到了什么${str},难道不是么？`; //键盘1左边那个键,反引号
    ```

4. 解构赋值

    ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）

    ```javascript
    // 以前写法
    let a = 1;
    let b = 2;
    let c = 3;

    //ES6写法
    let [a, b, c] = [1, 2, 3];


    ```


5. 数组

* ```forEach```

* ```map``` 返回值是一个数组

* ```filter```

6. 函数

* 函数参数默认值

    ```javascript
    function foo(age = 25,){ 
        // ...
    }
    ```
* Arrow箭头函数

7. 延展操作符

    ```javascript
    let a = [...'hello world']; 
    console.log(a);// ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
    ```

8. 对象属性简写

    ```javascript
    const name='小豪',
    const obj = { name };
    ```
9. ```Promise```

10. ```class```