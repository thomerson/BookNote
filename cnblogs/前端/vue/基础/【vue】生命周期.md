## vue 生命周期 

![生命周期](https://cn.vuejs.org/assets/lifecycle.16e4c08e.png)

new vue-> Init events & lifecycle -> beforeCreate -> beforeMount -> Mounted -> beforeUpdate -> updated -> beforeDestroyed -> destoryed


* beforeCreate

    在组件实例初始化完成之后立即调用。

    会在**实例初始化完成、props 解析之后**、**data() 和 computed 等选项处理之前**立即调用

* created

    在组件实例处理完所有与状态相关的选项后调用

* beforeMount

    在组件被挂载之前调用

    当这个钩子被调用时，组件已经完成了其响应式状态的设置，但**还没有创建 DOM 节点**

* mounted

    在组件被挂载之后调用

    <br>组件在以下情况下被视为已挂载：
    
    * 所有同步子组件都已经被挂载


    * 其自身的 DOM 树已经创建完成并插入了父容器中


* beforeUpdate

* updated

* beforeUnmount

    在一个组件实例**被卸载之前**调用

* unmounted

    在一个组件实例**被卸载之后**调用

### 生命周期中其他的钩子函数

* errorCaptured

    在捕获了**后代组件传递的错误时**调用

* renderTracked 

    仅在**开发模式下**可用

    在一个响应式依赖被组件的渲染作用追踪后调用


* renderTriggered

    仅在**开发模式下**可用

    在一个响应式依赖被组件触发了重新渲染之后调用


* activated

    若组件实例是 ```<KeepAlive>``` 缓存树的一部分，当组件被插入到 DOM 中时调用

* deactivated

    若组件实例是 ```<KeepAlive>``` 缓存树的一部分，当组件从 DOM 中被移除时调用

* serverPrefetch 

    当组件实例在服务器上被渲染之前要完成的异步函数


### created mounted说明

* created:在**模板渲染成html前**调用，即通常初始化某些属性值，然后再渲染成视图。
* mounted:在**模板渲染成html后**调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作,通常是在一些插件的使用或者组件的使用中进行操作，比如插件chart.js的使用

