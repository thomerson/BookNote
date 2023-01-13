## Pinia

状态管理

pinia和Vuex的作用是一样的，它也充当的是一个存储数据的作用，存储在pinia的数据允许我们在各个组件中使用。

## 和vuex比较

* pinia中只有state、getter、action，抛弃了Vuex中的Mutation，Vuex中mutation一直都不太受小伙伴们的待见，pinia直接抛弃它了，这无疑减少了我们工作量。

* pinia中action支持同步和异步，Vuex不支持

* 良好的Typescript支持



## 重要概念


* store

    data的仓库

    数据仓库的意思，我们数据都放在store里面。当然你也可以把它理解为一个公共组件，只不过该公共组件只存放数据，这些数据我们其它所有的组件都能够访问且可以修改

* state

    具体数据，类似data

    数据具体存在在哪里呢？前面我们利用defineStore函数创建了一个store，该函数第二个参数是一个options配置项，我们需要存放的数据就放在options对象中的state属性内。


* getter

    存放计算属性

    getters是defineStore参数配置项里面的另一个属性，前面我们讲了state属性。getter属性值是一个对象，该对象里面是各种各样的方法。大家可以把getter想象成Vue中的计算属性，它的作用就是返回一个新的结果，既然它和Vue中的计算属性类似，那么它肯定也是会被缓存的，就和computed一样。



* action 

    存放函数方法

    该属性就和我们组件代码中的methods相似，用来放置一些处理业务逻辑的方法。

    actions属性值同样是一个对象，该对象里面也是存储的各种各样的方法，包括同步方法和异步方法。