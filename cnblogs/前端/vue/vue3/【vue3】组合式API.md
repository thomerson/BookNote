## 组合式api

优点

**代码共享**，在 setup 钩子里面，可以按逻辑上的关注点来对代码进行分组，并与其他组件共享代码。

注意```setup```参数

* ```props```
    
    父组件传入的props

*  ```context``` 
    
    包含3个参数，attrs、slots，emit

```javascript

export default {
  props: {
    user: { type: String }
  },
  setup(props,context) {

    // props :父组件传入的props
    // context :包含3个参数，attrs、slots，emit
    console.log(props) // { user: '' }

    return {} // 这里返回的任何内容都可以用于组件的其余部分
  }
  // 组件的“其余部分”
}


```
