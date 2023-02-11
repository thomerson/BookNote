## 依赖注入

解决组件**prop逐级透传**的问题

provide/inject

父组件通过provide来提供变量, 然后在子组件中可以通过 inject来注入变量。但provide和inject数据不是响应的，改变的provide的数据，不会响应到inject注入的值

由于vue有$parent属性可以让子组件访问父组件。但孙组件想要访问祖先组件就比较困难。通过provide/inject可以轻松实现跨级访问祖先组件的数据


```javascript
// parent
{
  name:'parentComp'
  data(){reture {}},
  provide(){
      reture{
        isShowA:true  
      }
  }
}

// child
{
  name:'childComp',
  inject:['isShowA'],
  mouted(){
      this.isShowA=false
  }
}
```

在任意层级都能访问导致数据追踪比较困难

