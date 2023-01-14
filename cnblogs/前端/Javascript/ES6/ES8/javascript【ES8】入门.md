## ES8

1. async/await

    <br>异步终极解决方案
    <br>

    ```javascript
    async getData(){
        const res = await api.getTableData(); // await 异步任务
        // do something    
    }
    ```

2. ```Object.values()```

    <br>

    返回可枚举属性值的对象

    ```javascript
    Object.values({a: 1, b: 2, c: 3}); // [1, 2, 3]
    ```

2. ```Object.entries```

    <br>

    返回一个给定对象自身可枚举属性的键值对数组

    ```javascript
    Object.entries({a: 1, b: 2, c: 3}); // [["a", 1], ["b", 2], ["c", 3]]
    ```

3. String padding

    <br>

    字符串填充

    ```javascript
    // padStart
    'hello'.padStart(10); // "     hello"
    // padEnd
    'hello'.padEnd(10) "hello     "
    ```
4. ```SharedArrayBuffer```

    <br>

    用来表示一个通用的，固定长度的原始二进制数据缓冲区

5. Atomics对象

    <br>

    提供了一组静态方法用来对 SharedArrayBuffer 对象进行原子操作

