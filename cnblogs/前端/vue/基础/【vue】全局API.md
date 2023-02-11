## version

Vue 版本

## nextTick() 

等待下一次 DOM 更新刷新的工具方法⭐

 Vue 中更改响应式状态时，最终的 DOM 更新并不是同步生效的，而是由 Vue 将它们缓存在一个队列中，直到下一个“tick”才一起执行。这样是为了确保每个组件无论发生多少状态改变，都仅执行一次更新。

nextTick() 可以在状态改变后立即使用，以等待 DOM 更新完成

常用的场景是在进行获取数据后，需要对新视图进行下一步操作或者其他操作时，**发现获取不到 dom**。⭐

**因为赋值操作只完成了数据模型的改变并没有完成视图更新**。在这个时候我们需要用到 nextTick。

```javascript
Vue.component('example', {
    template: '<span>{{ message }}</span>',
    data: function() {
        return {
            message: '未更新'
        }
    },
    methods: {
        updateMessage: function() {
            this.message = '已更新'
            console.log(this.$el.textContent) // => '未更新'
            this.$nextTick(function() {
                console.log(this.$el.textContent) // => '已更新'
            })
        }
    }
})

//因为 $nextTick() 返回一个 Promise 对象，所以你可以使用新的 ES2017 async/await 语法完成
methods: {
    updateMessage: async function() {
        this.message = '已更新'
        console.log(this.$el.textContent) // => '未更新'
        await this.$nextTick()
        console.log(this.$el.textContent) // => '已更新'
    }
}
```
