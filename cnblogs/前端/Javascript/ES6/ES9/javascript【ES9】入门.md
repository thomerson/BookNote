## ES9

1. 异步迭代

    <br>await可以和for...of循环一起使用，以串行的方式运行异步操作
    <br>
    ```javascript
    async function process(array) {
        for await (let i of array) {
        // doSomething(i);
        }
    }
    ```

2. ```Promise.finally()```

    ```javascript
    Promise.resolve().then().catch(e => e).finally();
    ```

3. 正则表达式

4. 拓展运算符```Rest/Spread``` 属性

    <br>扩展运算符（spread）是三个点（…）

    <br>rest参数，形式为：...变量名

    <br>**功能是把数组或类数组对象展开成一系列用逗号隔开的值**

    ```javascript
    function add(x, y) {
        return x + y;
    }
    const numbers = [4, 38];
    add(...numbers) // 42
    ```
