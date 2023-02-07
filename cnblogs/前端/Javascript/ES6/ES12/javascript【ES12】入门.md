## ES12

1. ```replaceAll```

    <br>返回一个全新的字符串，所有符合匹配规则的字符都将被替换掉

    ```javascript
    const str = 'hello world';
    str.replaceAll('l', ''); // "heo word"
    ```

2. ```Promise.any```
    <br>收一个Promise可迭代对象，只要其中的一个 promise 成功，就返回那个已经成功的 promise 。如果可迭代对象中没有一个 promise 成功（即所有的 promises 都失败/拒绝），就返回一个失败的 promise

3. WeakRefs


4. 逻辑运算符和赋值表达式

    ```javascript
	a ||= b
	//等价于
	a = a || (a = b)
	
	a &&= b
	//等价于
	a = a && (a = b)
	
	a ??= b
	//等价于
	a = a ?? (a = b)
    ```

5. 数字分隔符

    可以在数字之间创建可视化分隔符，通过_下划线来分割数字，使数字更具可读性

    ```javascript
    const money = 1_000_000_000;
    //等价于
    const money = 1000000000;

    1_000_000_000 === 1000000000; // true
    ```
    