## ES11

1. ```??```空值处理

    <br>Nullish coalescing Operator

    ```javascript
	let user = {
		u1: 0,
		u2: false,
		u3: null,
		u4: undefined,
		u5: ''
	}
    let u1 = user.u1 ?? '用户1'  // 0
	let u2 = user.u2 ?? '用户2'  // false
	let u3 = user.u3 ?? '用户3'  // 用户3
	let u4 = user.u4 ?? '用户4'  // 用户4
	let u5 = user.u5 ?? '用户5'  // ''
    ```

2. ```?.```Optional chaining（可选链）

    <br>检测不确定的中间节点

    ```javascript
    let user = {}
    let u1 = user.childer.name // TypeError: Cannot read property 'name' of undefined
    let u1 = user.childer?.name // undefined
    ```

3. ```Promise.allSettled```


4. ```import()```

    <br>按需导入

5. 基本数据类型```BigInt```

    任意精度的整数

6. globalThis

* 浏览器：window
* worker：self
* node：global

