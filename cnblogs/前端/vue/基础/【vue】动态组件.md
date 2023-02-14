## 动态组件


### :is
* 限制元素
> dom的一些html元素对放入它里面的元素有限制
```html
<!-- 错误写法 -->
<ul>
  <my-component></mu-component>
<ul>

<!-- 正确 -->
<ul>
  <li is="my-component"></li>
</ul>

```
* 动态组件
```html
<component v-bind:is="currentTabComponent"></component>
```

### keep-alive
有时候，我们希望能保持被动态加载组件的状态，已**避免反复重复渲染**导致的性能问题。为了能实现保持组件状态的功能，我们可以用一个 <keep-alive> 的元素将其动态组件包裹起来。

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```
虽然用keep-alive会缓存组件，但是也同样带来了问题，就是当我们**第二次进入页面时，组件的created和mounted函数不再触发**
对于这一问题，有两个解决方案：

* 增加activated()函数,每次进入新页面的时候再获取一次数据
* 在keep-alive中增加一个过滤exclude：name（组件的name）这个方法适用于其中单个


### activated deactivated
* activated 触发时机：keep-alive组件激活时使用；
* deactivated 触发时机：keep-alive组件停用时调用；

### 隐式贯穿 fallthrough
只有单个根节点的子节点**自动绑定**attribute行为

### 非 Prop 的 Attribute
[非 Prop 的 Attribute](https://www.jianshu.com/p/a87e4f84c6b0)

在使用组件时，写了组件里并没有定义的props或者emits
age我们在组件里就没有定义过
那这样vue也是不会报错的


给组件增加一个非 Prop 的 Attribute
```html
<girl-friend name="石原里美" age="20" class="my-girl"></girl-friend>
```

组件渲染的结果
```html
<div age="20" class="my-girl">
  <h1>石原里美</h1>
</div>
```

#### inheritAttrs
那如果子组件不想要这些父组件传来的没有定义过的属性，就可以通过inheritAttrs来设置
```html
<template>
  <div>
    <h1>{{ name }}</h1>
  </div>
</template>
```

```javascript


<script>
export default {
  props: {
    name: {
      type: String,
      default: '',
    },
  },
  inheritAttrs: false, // 新增
}
</script>
```


