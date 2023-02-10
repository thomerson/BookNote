## vue table 修改数据不刷新

由于 JavaScript 的限制，Vue 不能检测数组的变动：

当你利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength

解决方法

1. 使用```set```或者```splice``` 推荐

```javascript
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

2. 设置后更新UI

作用：强制渲染，多层for循环嵌套时出现的页面没有及时刷新改变后的值的问题
解决页面 v-for 中修改 item 属性值后 view 没有同步更新

```javascript
this.$forceUpdate()
```
