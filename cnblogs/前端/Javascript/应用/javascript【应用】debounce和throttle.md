## ```debounce```防抖

在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时；典型的案例就是输入搜索：输入结束后n秒才进行搜索请求，n秒内又输入的内容，就重新计时。

### debounce使用场景
* scroll事件（资源的加载）
* mousemove事件（拖拽）
* resize事件（响应式布局样式）
* keyup事件（输入框文字停止打字后才进行校验）



```html
<div id="app">
    输入内容：<input type="text" class="input"  @keyup="deb"/>
    <div> 输入次数：{{ num }}</div>
</div>
```

```javascript
let time
var app = new Vue({
    el: '#app',
    data: {
        num: 0,
    },
    methods: {
        deb: function () {
            let that = this
            if (time) {
                clearTimeout(time)
            }
            time = setTimeout(function () {
                that.num++
                console.log('输入了' + that.num + '次')
                time = undefined;
            }, 2000)
        }
    })

```


## ```throttle```节流


规定在一个单位时间内，只能触发一次函数，如果这个单位时间内触发多次函数，只有一次生效； 典型的案例就是鼠标不断点击触发，规定在n秒内多次点击只有一次生效。

在某应用购买火车票/飞机票等商品的时候，不断地点击刷新购买，总不能一直点击一直请求吧，系统会崩掉的，所以节流就很有必要了。

### throttle使用场景
* click事件（不停快速点击按钮，减少触发频次）
* scroll事件（返回顶部按钮出现\隐藏事件触发）
* keyup事件（输入框文字与显示栏内容复制同步）
* 减少发送ajax请求，降低请求频率


```html

<div id="app">
    <button @click="thr">点击</button>
    <div>实际点击：{{clicknumber}}</div>
    <div>有效点击：{{num}}</div>
</div>
```


```javascript
let time
let lastTime
var app = new Vue({
    el: '#app',
    data: {
        num: 0,
        clicknumber: 0
    },
    methods: {
        thr: function () {
            this.clicknumber++
            let that = this
            let now = new Date();
            if (lastTime && now - lastTime < 2000) {
                clearTimeout(time)
            }
            time = setTimeout(function () {
                that.num++
                console.log('点击了' + that.num + '次')
                lastTime = new Date()
            }, 500)
        }
    }
})


```


## 区别

防抖动是将多次执行变为**最后一次执行**

节流是将多次执行变成**每隔一段时间执行**


## Lodash实现

[vue中使用Lodash实现debounce和throttle]()



