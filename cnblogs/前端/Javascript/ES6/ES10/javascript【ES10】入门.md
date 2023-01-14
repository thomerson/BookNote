## ES10

1. 数组

* flat
    <br>展平
    ```javascript
    [1, 2, [3, 4]].flat(Infinity); // [1, 2, 3, 4]
    ```

* flatMap

    <br>展平
    ```javascript
    [1, 2, 3, 4].flatMap(a => [a**2]); // [1, 4, 9, 16]
    ```

2. 字符串

* ```String.trimStart()```

* ```String.trimEnd()```

* ```String.prototype.matchAll```

    <br>为所有匹配的匹配对象返回一个迭代器

3. ```Symbol.prototype.description```

    <br>只读属性，回 Symbol 对象的可选描述的字符串

    ```javascript
    Symbol('description').description; // 'description'
    ```

4. ```Object.fromEntries()```

    <br>返回一个给定对象自身可枚举属性的键值对数组

    和```Object.entries```相反操作

    ```javascript
    // 通过 Object.fromEntries， 可以将 Map 转化为 Object:
    const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
    console.log(Object.fromEntries(map)); // { foo:     "bar", baz: 42 }
    ```
