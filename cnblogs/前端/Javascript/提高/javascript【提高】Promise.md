## Promise

ES6新语法，用来**优化异步代码**的写法

在没有Promise之前，javascript中的异步处理，大多是利用**回调函数**来实现的

### 使用方式

```javascript
var p1 = new Promise(function(resolve,reject){
     //异步操作 resolve(obj1)  或者 reject(obj2)
});
p1.then(function(rs){
    // 如果p1的状态是resolved，则then中的函数
    //会执行，且obj1的值会传给rs
}).catch(function(rs){
    // 如果p1的状态是reject，则catch中的函数
    //    会执行，且obj2的值会传给rs
}).finally(function(){
    // 一定会执行的函数
})

```

### 构造器

```js
// const obj = new Object()
// const arr = new Array()
const p1 = new Promise(function(resolve,reject){
  // 执行异步代码
  // 调用resolve,或者reject
});
console.dir(p1)
```

⭐ 构造器必须要给定一个参数

⭐ 构造器的实参是一个函数，这个函数有两个形参(resolve,reject)，这两个形参也是函数

⭐ 在函数体的内部， 一般会执行异步代码，然后根据情况来调用resolve()或者是reject()

### 三种状态

1. 初始态```pending```

 创建Promise对象时，且没有调用resolve或者是reject方法，相当于是初始状态。
 
 这个初始状态会随着你调用resolve，或者是reject函数而切换到另一种状态

 2. 成功态```resolved```

 表示解决了，承诺实现了

 3. 失败态```rejected```

 表示拒绝，失败，承诺没有做到

⭐ 状态是可转化

```js
 pending -----  resolve() --> resolved;
 pending -----  reject()  --> rejected;
```

⭐ 状态转换不可逆

一旦从pending —> resolved（或者是rejected），就不可能再回到pending，也不能由resolved变成rejected

### 改写回调函数

ajax

```js
function get(success){
    const xhr=XMLHttpRequest();
    xhr.open('get','/api/values');
    xhr.onreadystatechange=function(){
        if(this.readState===4){
            const data=JSON.parse(xhr.responseText);
            
            // 回调
            success(data);
        }
    };
    xhr.send();
}

```

使用promise

```js
function get(){
    return new Promise((resolve,reject)=>{
        const xhr=XMLHttpRequest();
         xhr.open('get','/api/values');
        xhr.onreadystatechange=function(){
            if(this.readState===4){
                const data=JSON.parse(xhr.responseText);

                // resolve
                resolve(d);
            }
        }
    });
    xhr.send();
}

```

### then

then方法的作用是为Promise对象添加状态改变时的回调函数

```js
// p 是一个promise对象
p.then(函数1[,函数2])
```

* 第一个参数是resolved状态的回调函数。当p的状态从pending变成了resolved，函数1会执行。
* 第二个参数是rejected状态的回调函数。 当p的状态从pending变成了rejected，函数2会执行

⭐ 第二个参数是可选的

```js
var p = new Promise((resolve,reject)=>{
   //  resolve(val1);
   reject(val2)
})

p.then(
  	okVal=>{ // resolved时执行
    	console.info("成功");
    	console.log(okValue);
		}, 
  	errVal=>{ // rejected时执行
    	console.info("失败");
    	console.log(errValue);
    }
)

```

💡 then()方法的**返回值是一个promise对象**，所以支持**链式**写法。但是要注意的是它的返回值是一个新的promise对象，与调用then方法的并不是同一个对象。

```js
var p1 = new Promise(()=>{});
var p2 = p1.then(function f_ok(){}, function f_err(){}); 
// p2也是一个promise对象。

console.log(p1 === p2); // false
```

### catch


catch 是 ```then(null, reject)的```别名

```js
var p1 = new Promise((resolve,reject)=>{
	reject('s')
});

p1.catch(function(err){
	console.log(err);
})

// 与下面的代码等价
p1.then(null, function(err){
	console.log(err);
})

```


### promise的链式调用

```js
function do1() {
    console.log("任务1");
}
function do2() {
    console.log("任务2");
}
function do3() {
     console.log("任务3");
}
function do4() {
    console.log("任务4");
}

var p = new Promise((resolve,reject)=>{ resolve()})
p.then(do1)
 .then(do2)
 .then(do3)
 .then(do4);

```


### 笔试题

让sleep 的功能与setTimeout一样：就是等2000毫秒之后再执行后续操作

```js
function sleep(time){
    // 请写出你的代码
}

sleep(2000).then(()=>{
    console.log("后续操作")
})
console.log(2);
```


