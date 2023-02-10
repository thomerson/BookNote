## 事件修饰符

* ```.stop```

    阻止冒泡

* ```.prevent```

    阻止默认事件

* ```.capture```

    添加事件侦听器时使用事件捕获模式 

    即是给元素添加一个监听器，当元素发生冒泡时，先触发带有该修饰符的元素。若有多个该修饰符，则由外而内触发。


* ```.self```

    当事件在该元素本身（比如不是子元素）触发时触发回调

* ```.once``` 

    点击事件将只会触发一次

* ```.passive```

    一般用于触摸事件的监听器，可以用来改善移动端设备的滚屏性能

    每次事件产生，浏览器都会去查询一下是否有preventDefault阻止该次事件的默认动作。加上passive就是为了告诉浏览器，不用查询了，没用```preventDefault```阻止默认动作。

    请勿同时使用 ```.passive``` 和 ```.prevent```，因为 ```.passive``` 已经向浏览器表明了你不想阻止事件的默认行为。

    一般用在滚动监听，@scoll，@touchmove ,可以大大提升滑动的流畅度


```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```


### 使用注意

1. ```prevent.self```和```self.prevent```

* v-on:click.prevent.self

    会阻止所有的点击

* v-on:click.self.prevent

    只会阻止对元素自身的点击



## 按键修饰符

* ```.enter```
* ```.tab```
* ```.delete``` (捕获“Delete”和“Backspace”两个按键)
* ```.esc```
* ```.space```
* ```.up```
* ```.down```
* ```.left```
* ```.right```

### 系统按键修饰符

* ```.ctrl```
* ```.alt```
* ```.shift```
* ```.meta``` 
    在 Mac 键盘上，meta 是 Command 键 (⌘)。在 Windows 键盘上，meta 键是 Windows 键 (⊞)

### ```.exact``` 修饰符

允许控制触发一个事件所需的确定组合的系统按键修饰符

```html
<!-- 当按下 Ctrl 时，即使同时按下 Alt 或 Shift 也会触发 -->

<button @click.ctrl="onClick">A</button>

<!-- 仅当按下 Ctrl 且未按任何其他键时才会触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 仅当没有按下任何系统按键时触发 -->
<button @click.exact="onClick">A</button>
```

### 鼠标按键修饰符

* ```.left```
* ```.right```
* ```.middle```

