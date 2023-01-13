## ES6

```ES2015```

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