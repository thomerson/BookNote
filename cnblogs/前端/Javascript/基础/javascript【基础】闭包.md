## 闭包


**能够访问另一个函数作用域的变量的函数**

闭包就是一个函数，这个函数能够访问其他函数的作用域中的变量。


当一个内部函数被调用，就会形成闭包，闭包就是能够读取其他函数内部变量的函数。

* 作用

局部变量无法共享和长久的保存，而全局变量可能造成变量污染，所以我们希望有一种机制既可以长久的保存变量又不会造成全局污染。

* 缺点

如果不是某些特定任务需要使用闭包，在其它函数中创建函数是不明智的，因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响。



```javascript

var inc = (function () { // 该函数体中的语句将被立即执行
  var count = 0; // 局部变量 count 初始化
  return function () { // 父函式返回一个闭包（函式引用）
    return ++count; // 当父函式 return（即上一个 return）后，这里的 count 不再是父函式的局部变量，而是返回结果闭包中的一个闭包（环境）变量。
  };
}) ();

inc(); // return: 1
inc(); // return: 2

```


```javascript
// 传统oop
var obj = {
  count: 0,
  inc: function () {
    return ++this.count;
  }
};
obj.inc(); // count: 1
obj.inc(); // count: 2

```


经典闭包笔试题

```js
function func(n,o){
    console.log(n,o);
    return{
        func:function(m){
            return func(m,n);
        }
    };
}
var a = func(0);
a.func(1);
a.func(2);
a.func(3);

var b = func(0).func(1).func(2).func(3);

var c = func(0).func(1);
c.func(2);
c.func(3);

```

输出结果

```js
var a = func(0);//0 undefined
a.func(1);//1 0 
a.func(2);//2 0 
a.func(3);//3 0

var b = func(0).func(1).func(2).func(3);//0 undefined  , 1 0 , 2 1 , 3 2

var c = func(0).func(1);//0 undefined;1 0
c.func(2);//2 1
c.func(3);//3 1
```

