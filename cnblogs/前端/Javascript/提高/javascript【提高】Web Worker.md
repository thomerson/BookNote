## Web Worker

允许我们在 js 主线程之外开辟新的 Worker 线程，并将一段 js 脚本运行其中，它赋予了开发者利用 js **操作多线程**的能力。

1. 检测浏览器是否支持web worker

    *IE10以下不支持web worker*

```javascript
if(typeof(Worker)!=="undefined")
{
    // 是的! Web worker 支持!
    // 一些代码.....
}
else
{
    //抱歉! Web Worker 不支持
}
```

2. 创建 web worker 文件

新建```demo_workers.js```

```javascript
var i=0;

function timedCount()
{
    i=i+1;
    postMessage(i); // 向主线程传递参数
    setTimeout("timedCount()",500);
}

timedCount();

// 接收主线程消息
self.addEventListener('message', e => { 
    console.loworker线程g(e.data); // 主线程发送的消息
    self.postMessage('Greeting from Worker.js'); // 向主线程发送消息
});

// worker线程 监听错误信息
self.addEventListener('error', err => { // 当worker内部出现错误时触发
    console.log(err.message);
});
self.addEventListener('messageerror', err => { //当 message 事件接收到无法被反序列化的参数时触发
    console.log(err.message);
});


```

3. 创建 Web Worker 对象

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
 
<p>计数： <output id="result"></output></p>
<button onclick="startWorker()">开始工作</button> 
<button onclick="stopWorker()">停止工作</button>
 
<p><strong>注意：</strong> Internet Explorer 9 及更早 IE 版本浏览器不支持 Web Workers.</p>
 
<script>
var w;
 
function startWorker() {
    if(typeof(Worker) !== "undefined") { //判断浏览器是否支持web worker
        if(typeof(w) == "undefined") {
            w = new Worker("demo_workers.js"); // 创建 Web Worker 对象

            w.postMessage();// 向 worker 线程发送消息，对应 worker 线程中的 e.data
        }
        w.onmessage = function(event) { // 主线程监听worker线程消息
            document.getElementById("result").innerHTML = event.data;
        };

        // 监听错误信息
        w.addEventListener('error', err => {//当worker内部出现错误时触发
            console.log(err.message); 
        });
        w.addEventListener('messageerror',err => { //当 message 事件接收到无法被反序列化的参数时触发
            console.log(err.message)
})


    } else {
        document.getElementById("result").innerHTML = "抱歉，你的浏览器不支持 Web Workers...";
    }
}
 
function stopWorker() 
{ 
    w.terminate(); //终止 Web Worker
    w = undefined;
}
</script>
 
</body>
</html>
```

4. Worker 线程引用其他js文件

使用```importScripts```

```javascript
// utils.js
const add = (a, b) => a + b;
```

```javascript
// worker.js（worker线程）
// 使用方法：importScripts(path1, path2, ...); 

importScripts('./utils.js');

console.log(add(1, 2)); // log 3


```

## 注意

Web Workers **无法操作 DOM**

由于 web worker 位于外部文件中，它们无法访问下列 JavaScript 对象：

* window 对象
* document 对象
* parent 对象
