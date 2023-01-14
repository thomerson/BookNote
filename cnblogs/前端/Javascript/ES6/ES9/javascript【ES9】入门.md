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

