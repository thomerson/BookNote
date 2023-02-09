## DOM模型

![DOM模型](https://upload-images.jianshu.io/upload_images/1620777-dafb1778461af7d9.png?imageMogr2/auto-orient/strip|imageView2/2/w/698/format/webp)

### 事件捕获

事件从最上一级标签开始往下查找，直到捕获到事件目标(target)。和事件冒泡相反，事件捕获是自上而下执行。


### 事件冒泡

事件冒泡的流程刚好是事件捕获的逆过程，事件从事件目标(target)开始，自下而上冒泡直到页面的最上一级标签.

事件冒泡允许多个操作被**集中处理**（把事件处理器添加到一个父级元素上，避免把事件处理器添加到多个子级元素上），它还可以让你在对象层的不同级别捕获事件

* 阻止事件冒泡```event.stopPropagation()```

    IE不支持,设置```cancelBubble```属性

    ```javascript
     if(e && e.stopPropagation){  
        e.stopPropagation();  
    } else {  
        window.event.cancelBubble = true;  
    }  
    ```

### 默认行为

浏览器的一些默认的行为。例如：点击超链接跳转，点击右键会弹出菜单，滑动滚轮控制滚动条等


* 阻止默认行为```event.preventDefault()```

    IE不支持,设置```returnValue```属性

    ```javascript
       if(e && e.preventDefault) {  
            e.preventDefault();  
        } else {  
            window.event.returnValue = false;  
        }  
        return false;  
    ```


### event中target和currentTarget

在事件处理程序内部，对象this始终等于currentTarget的值，而target则只包含事件的实际目标。

* 如果直接将事件处理程序指定给了目标元素，则this、currentTarget和target包含相同的值。

```javascript
var btn = document.getElementById("myBtn");
btn.onclick = function (event) {
    alert(event.currentTarget === this); //ture
    alert(event.target === this); //ture
};

```

* 如果事件处理程序存在于按钮的父节点中，那么这些值是不相同的。


```javascript
document.body.onclick = function (event) {
    alert(event.currentTarget === document.body); //ture
    alert(this === document.body); //ture
    alert(event.target === document.getElementById("myBtn")); //ture
};

```

