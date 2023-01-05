## 变量提升

全局作用域中声明的变量会提升至全局最顶层，函数内声明的变量只会提升至该函数作用域最顶层

 
```javascript
console.log(a);
var a = "a";
var foo = () => {
    console.log(a);
    var a = "a1";
}
foo();
```

实际运行时

```javascript
var a;
console.log(a); // undefined
a = "a";
var foo = () => {
    var a; // 全局变量会被局部作用域中的同名变量覆盖
    console.log(a); // undefined
    a = "a1";
}
foo();
```

## 函数提升

**函数提升只会提升函数声明，而不会提升函数表达式**


```javascript
console.log(foo1); // [Function: foo1]
foo1(); // foo1
console.log(foo2); // undefined
foo2(); // TypeError: foo2 is not a function
function foo1 () {
    console.log("foo1");
};
var foo2 = function () {
    console.log("foo2");
};
```

