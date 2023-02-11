## Vuex

专门为vue.js设计的集中式状态管理架构。


1. 安装

```node
npm n install vuex --save
```

2. 使用

    1. 新建一个vuex文件夹（这个不是必须的），并在文件夹下新建store.js文件，文件中引入我们的vue和vuex

    ```javascript
    import Vue from 'vue';
    import Vuex from 'vuex';
    ```

    2. 使用vuex

    ```javascript
    Vue.use(Vuex);
    ```

## 基本属性

* state

    基本数据(数据源存放地)

* Getter

    从基本数据派生出来的数据，类似计算属性

* Mutation

    提交更改数据的**方法**，**同步**

    vue3中Pinia取消了Mutation

* Action

    像一个装饰器，包裹mutations，使之可以**异步**。

* Module

    模块化Vuex
    
