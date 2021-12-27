## 说明
记录vue学习和使用过程中遇到的一些问题，不定期更新

## vue 生命周期 created mounted

new vue-> Init events & lifecycle -> beforeCreate -> beforeMount -> Mounted -> beforeUpdate -> updated -> beforeDestroyed -> destoryed

* created:在**模板渲染成html前**调用，即通常初始化某些属性值，然后再渲染成视图。
* mounted:在**模板渲染成html后**调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作,通常是在一些插件的使用或者组件的使用中进行操作，比如插件chart.js的使用

## vue table 修改数据不刷新

由于 JavaScript 的限制，Vue 不能检测以下数组的变动：

当你利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength

解决方法

```javascript
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

## vue forceUpdate
作用：强制渲染，多层for循环嵌套时出现的页面没有及时刷新改变后的值的问题
解决页面 v-for 中修改 item 属性值后 view 没有同步更新

```javascript
this.$forceUpdate()
```

## vue :key

* v-if中用 key 管理可复用的元素
Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染

* v-for中的key
    建议尽可能使用 v-for 来提供 key ，除非迭代 DOM 内容足够简单，或者你是故意要依赖于默认行为来获得性能提升

    - 虚拟 DOM 的 Diff 算法
    - 利用 key 的唯一性生成 map 对象来获取对应节点，比遍历方式更快, 指定 key 后, 可以保证渲染的准确性(尽可能的复用 DOM 元素。)

## computed & watch
* watch 需要在数据变化时**执行异步**或**开销较大**的操作时
* computed 计算属性是基于它们的反应依赖关系缓存的,计算属性只在相关响应式依赖发生改变时它们才会重新求值

### watch 深度监控
```javascript
watch: {
    'cityName.name': {
      handler(newName, oldName) {
      // ...
      },
      deep: true,
      immediate: true
    }
  }
```

## vue for 渲染控件 数组多层嵌套校验

注意prop和rules写法

```html
<div v-for="(site,siteIndex) in tempProject.tempSampleSite" :key="'site-'+siteIndex">
  <el-row>
    <el-col :span="12">
      <el-form-item
        label-width="30%"
        label="名称"
        :prop="'tempSampleSite.'+siteIndex+'.name'"
        :rules="{ required: true, message: '取样部位不能为空', trigger: 'change' }"
      >
        <el-input v-model="site.name" :maxlength="40" />
      </el-form-item>
    </el-col>
    <el-col :span="12">
      <el-button
        v-show="siteIndex==tempProject.tempSampleSite.length-1"
        type="text"
        size="mini"
        style="margin-left: 20px;"
        @click="addSampleSite"
      >
        添加
      </el-button>
    </el-col>
  </el-row>
</div>
```

## vue $nextTick()

Vue 在更新 DOM 时是异步执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部对异步队列尝试使用原生的 Promise.then、MutationObserver 和 setImmediate，如果执行环境不支持，则会采用 setTimeout(fn, 0) 代替。

常用的场景是在进行获取数据后，需要对新视图进行下一步操作或者其他操作时，**发现获取不到 dom**。
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

---

## vue Object.defineProperty

当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，并使用 ```Object.defineProperty``` 把这些 property 全部转为 getter/setter。Object.defineProperty 是 ES5 中一个无法 shim 的特性，这也就是 Vue 不支持 IE8 以及更低版本浏览器的原因。

## vue select
### select option绑定复杂对象
```html
<select v-model="selected">
  <!-- 内联对象字面量 -->
  <option :value="{ number: 123 }">123</option>
</select>
```

### vue select option list 被修改后，无法选中

原因：还是 vue 无法监视数组改变
解决方案：使用```forceUpdate```强制更新dom

```html
<el-select v-model="listQuery.samplePointId" placeholder="科室" style="width: 20%" @change="samplePointChange">
  <el-option v-for="item in samplePointList" :key="item.id" :label="item.name" :value="item.id" />
</el-select>
```

```javascript
methods: {
    samplePointChange() {
        this.$forceUpdate()
    }
}
```

## vue风格指南
[风格指南](https://v3.cn.vuejs.org/style-guide/#%E8%A7%84%E5%88%99%E7%B1%BB%E5%88%AB)

## filter

## route

### route跳转


### 获取route中的query参数
```javascript
var query=this.$route.query;

let lat = query.lat;
```

## slot

### slot备用内容
只会在没有提供内容的时候被渲染
将它放在 <slot> 标签内


### 具名插槽
一个不带 name 的 <slot> 出口会带有隐含的名字“default”

### slot作用域
**插槽内容无法直接访问子组件中的数据**
解决方法：

### 解构插槽 Prop

## mixin 混入

<!-- TODO -->

## 组合式 API

<!-- TODO -->

## emit

### 验证emit的事件
```javascript
app.component('custom-form', {
  emits: {
    // 没有验证
    click: null,

    // 验证 submit 事件
    submit: ({ email, password }) => {
      if (email && password) {
        return true
      } else {
        console.warn('Invalid submit event payload!')
        return false
      }
    }
  },
  methods: {
    submitForm(email, password) {
      this.$emit('submit', { email, password })
    }
  }
})
```


### 修改组件v-model的值
```html
<user-name
  v-model:first-name="firstName"
  v-model:last-name="lastName"
></user-name>
```

```javascript
app.component('user-name', {
  props: {
    firstName: String,
    lastName: String
  },
  emits: ['update:firstName', 'update:lastName'],
  template: `
    <input 
      type="text"
      :value="firstName"
      @input="$emit('update:firstName', $event.target.value)">

    <input
      type="text"
      :value="lastName"
      @input="$emit('update:lastName', $event.target.value)">
  `
})
```

## props

<!-- TODO -->

## stylus

<!-- TODO -->

## vue i18n

[vue-i18n](https://kazupon.github.io/vue-i18n/)






## vue el-checkbox 单选

```html
<el-checkbox-group v-model="habitation" class="el-checkbox-inline">
  <el-checkbox
    v-for="item in dict.habitation"
    :label="item.value"
    :key="item.value"
    @change="checkboxChange('habitation')"
    >{{item.label}}</el-checkbox
  >
</el-checkbox-group>
```

```javascript
export default {
data() {
 dict:{
     habitation:[{'label':'城市','value':1},{'label':'农村','value':2}]
 },
 habitation: [],
      model: {
          'habitation':undefined
      }
},
methods:{
    checkboxChange(name) {
      if (Array.isArray(this.$data[name]) && this.$data[name].length > 1) {
        this.$data[name].splice(0, 1)
      }
      console.log(this.$data[name], name)
    }
}
}
```

## 清空 npm 缓存
```shell
npm cache clean -f
```


## idea 按照eslint格式化
[IDEA 配置ESlint](https://blog.csdn.net/zysen1995/article/details/107035499/)

```javascript
 switchingComponents(item){
                this.informationContent = item;

                console.log(this.edcFileInformationId,'edcFileInformationId')
                // if(item.state == false){
                //     this.$notify({
                //         title: '',
                //         message: '请先完成档案信息录入，才可以操作！',
                //         type: 'warning',
                //         duration: 2000
                //     });
                //     return;
                // }
                if(item.children.length > 0){
                   return;
                }
                this.informationId = item.id;
                this.informationComponent = item.component;
                this.switchKey = Date.parse( new Date());
                //console.log(this.currentKey,'this.currentKey')
            }
```

## element-ui单选框

```javascript
<el-radio label="1">药物涂层支架</el-radio>
```

bool类型
如果需要绑定数值或者布尔类型的值，需要在label前加上:

```javascript
<el-radio :label="true">是</el-radio>
```

### radio绑定model的特殊写法
```javascript
<input type="radio" v-model="pick" v-bind:value="a" />
```

## element-ui checkbox 0/1绑值
v-model加载时无法直接选中，需要用checked去帮助默认选中
```html
 <el-checkbox v-model="scope.row.noInspectionRecord" true-label="1" false-label="0" :checked="scope.row.noInspectionRecord==1" />
```


## element-ui checkboxgroup 不支持对象数组

-> 不用checkboxgroup v-for 一个一个去绑定

## element-ui rule required动态判断
不能写在scripts中,无效，只能第一次启用，被修改后无效

```javascript
rules: {
        patientName: [
          { required: this.isApp, message: '请输入姓名', trigger: 'blur' }
        ]
}
```

需要写在form-item中
```html
<el-form-item
                label="移植类型"
                prop="planCode"
                :rules="{required:this.typeId===1,message:'请选择移植类型',trigger: 'change'}"
              >
                <el-select v-model="dto.planCode" style="width:100%" placeholder="请选择">
                  <el-option
                    v-for="item in migrationTypeList"
                    :key="item.id"
                    :label="item.transplantName"
                    :value="item.id"
                  />
                </el-select>
                <!-- <el-input v-model="form.name"></el-input> -->
              </el-form-item>
```

### elementUI dialog设置高度
dialog只有设置width的属性，无法直接设置高度
需要添加自定义class,但是style由于添加了scope无法添加对应的样式
需要添加deep
```html
    <el-dialog
      v-if="showSampleInfoEdit"
      title="样本详情编辑"
      :visible.sync="showSampleInfoEdit"
      :close-on-click-modal="false"
      width="720px"
      custom-class="patientInformation"
    >
      <PatientInformation ref="patientInformation" />
      <span slot="footer" class="dialog-footer" style="padding-right: 20px;">
        <el-button @click="showSampleInfoEdit = false">取 消</el-button>
        <el-button :loading="sampleInfoEditLoading" type="primary" @click="saveSampleInfo">保 存</el-button>
      </span>
    </el-dialog>

```


```css
<style>
  /deep/ .patientInformation {
    height: 90vh;
    overflow: auto;
  }
</style>
```


#### deep使用
deep使用编译器会报错，无法启动项目
原因：因为使用了less或scss的原因
解决方法：使用::v-/deep/或者>>> 来替换/deep/
```css
<style>
  ::v-deep .el-icon-star-on{
    color: #7cb8f0;
  }
</style>

```



## vue rule 对象校验
```html
 <el-form :model="model" :rules="modelRules">
    <el-form-item prop="information.name" label="方案名称">
                  <el-input v-model="model.information.name" />
                </el-form-item>
                 <el-form-item prop="information.code" label="方案code">
                  <el-input v-model="model.information.code" />
                </el-form-item>
 </el-form>
```
```javascript
 model: {
        information: {
          id: 0,
          code: null,
          name: null,
          recommendedExclusionCriteria: null,
          recommendedInclusionCriteria: null,
          researchCycle: null,
          researchObjective: null
        },
        drugs: [],
        researchProcesses: [],
        researchGroups: []
      },
modelRules: {
        'information.name': [
          { required: true, message: '方案名称不能为空', trigger: 'change' }
        ],
        'information.code': [
          { required: true, message: '方案code不能为空', trigger: 'change' }
        ]
      }
```

或者rules写成
```javascript
modelRules: {
        information: {
          name: [{ required: true, message: '方案名称不能为空', trigger: 'change' }],
          code: [{ required: true, message: '方案code不能为空', trigger: 'change' }]
        }
}
```
## 组件


### vue 父子组件传值

* 父传子props
**单向下行绑定**,**不应该在一个子组件内部改变 prop**,需要修改的话在data中定义一个对象，并将prop作为初始值
```html
// 父组件
<KeyIndicators :patient-information-id="patientInformationId" />
```

```javascript
// 父组件
import KeyIndicators from './components/sampleAddition/KeyIndicators.vue'

// 子组件
props: {
    patientInformationId: {
      type: Number,
      default: 0
    }
  }
```

  * props 不同类型的写法
```javascript
refAge: {
type: Number,
default: 0
},
refName: {
type: String,
default: ''
},
hotDataLoading: {
type: Boolean,
default: false
},
hotData: {
type: Array,
default: () => {
return []
}
},
getParams: {
type: Function,
default: () => () => {}
},
meta: {
type: Object,
default: () => ({})
}
```

* 子传父$emit

```html
// 父组件
<PatientInformation @nextStep="next" />
```

```javascript
// 父组件
import PatientInformation from './components/sampleAddition/PatientInformation.vue'

// 父组件
methods: {
    next(val){
      console.log(val)
    }
  }


// 子组件
methods: {
    save(){
     this.$emit('nextStep', 'val')
    }
  }
```







### 父组件调用子组件方法
```html
<!-- 子组件ref给父组件 -->
 <PatientInformation ref="patientInformation" />
```

```javascript
import PatientInformation from './components/sampledetail/PatientInformation'
components: {
    PatientInformation: PatientInformation
  },
  methods:{
    test(){
       this.$refs.patientInformation.getModel(this.patientInformationId)
    }
  }
```




### 在组件上使用 v-model
[在组件上使用-v-model](https://v3.cn.vuejs.org/guide/component-basics.html#在组件上使用-v-model)
### $attrs
子组件有多个根元素时选择父组件传过来的class时使用，参考[组件属性继承](#组件属性继承)

### 组件属性继承


## vue防抖和节流
> 让某个函数在一定**事件间隔条件**（去抖debounce） 或**时间间隔条件**（节流throttle） 下才会去执行，避免快速多次执行函数（操作DOM，加载资源等等）给内存带来大量的消耗从而一定程度上降低性能问题
* debounce/防抖
  > 当调用动作n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间
* throttle/节流
  > 预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。

### debounce使用场景
* scroll事件（资源的加载）
* mousemove事件（拖拽）
* resize事件（响应式布局样式）
* keyup事件（输入框文字停止打字后才进行校验）


### throttle使用场景
* click事件（不停快速点击按钮，减少触发频次）
* scroll事件（返回顶部按钮出现\隐藏事件触发）
* keyup事件（输入框文字与显示栏内容复制同步）
* 减少发送ajax请求，降低请求频率


### Lodash
[如果某个组件仅使用一次，可以在 methods 中直接应用防抖](https://v3.cn.vuejs.org/guide/data-methods.html#防抖和节流)

[debounce](https://www.lodashjs.com/docs/lodash.debounce)
[throttle](https://www.lodashjs.com/docs/lodash.throttle)

```javascript
<script src="https://unpkg.com/lodash@4.17.20/lodash.min.js"></script>
<script>
  Vue.createApp({
    methods: {
      // 用 Lodash 的防抖函数
      click: _.debounce(function() {
        // ... 响应点击 ...
      }, 500)
    }
  }).mount('#app')
</script>
```

但是，这种方法对于可复用组件有潜在的问题，因为它们都共享相同的防抖函数。为了使组件实例彼此独立，可以在生命周期钩子的 created 里添加该防抖函数:
```javascript
app.component('save-button', {
  created() {
    // 用 Lodash 的防抖函数
    this.debouncedClick = _.debounce(this.click, 500)
  },
  unmounted() {
    // 移除组件时，取消定时器
    this.debouncedClick.cancel()
  },
  methods: {
    click() {
      // ... 响应点击 ...
    }
  },
  template: `
    <button @click="debouncedClick">
      Save
    </button>
  `
})
```

### Underscore


## CLI
[Cli](https://cli.vuejs.org/zh/)


## Vite
[Vite](https://cn.vitejs.dev/)


## vue指令

### v-if v-show
一般来说，v-if有更高的切换消耗，而v-show有更高的初始渲染消耗。因此，如果需要频繁切换，则使用v-show更好

### v-on v-bind v-text v-model
* v-on 绑定事件 简写@
  * 优点：简介清晰，DOM解耦，不需要清理事件
* v-bind 用来绑定文本或属性，绑定文本跟v-text差不多；能够绑定的属性有html中的属性、css的样式、对象、数组、number 类型、bool类型
        v-bind是单向绑定，它在绑定文本时，当data中的数据发生变化时，页面中的数据也相应改变，但页面中的数据改变不会影响到data
        缩写可以省略
* v-model 数据的双向绑定


### v-html


### v-once




## slot 插槽



## 修饰符

### v-model 修饰符
* .lazy 在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步
* .number 自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符
* .trim 自动过滤用户输入的首尾空白字符

### 事件修饰符
* .stop 阻止冒泡（通俗讲就是阻止事件向上级DOM元素传递）
* .prevent 阻止默认事件的发生 比如点击超链接的时候会进行页面的跳转，点击表单提交按钮时会重新加载页面等，使用".prevent"修饰符可以阻止这些事件的发生
* .capture 捕获冒泡，即有冒泡发生时，有该修饰符的dom元素会先执行，如果有多个，从外到内依次执行，然后再按自然顺序执行触发的事件
* .self 将事件绑定到自身，只有自身才能触发，通常用于避免冒泡事件的影响
* .once 设置事件只能触发一次，比如按钮的点击
* .passive [执行默认方法](https://blog.csdn.net/qq_33706382/article/details/90752362)
    > **passive和prevent冲突，不能同时绑定在一个监听器上**



### 按键修饰符
.enter => // enter键
.tab => // tab键
.delete (捕获“删除”和“退格”按键) => // 删除键
.esc => // 取消键
.space => // 空格键
.up => // 上
.down => // 下
.left => // 左
.right => // 右


### 系统修饰键
.ctrl
.alt
.shift
* .meta 
  * 在 Mac 系统键盘上，meta 对应 command 键 (⌘)
  *  Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)

### .exact 修饰符
```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

### 鼠标按钮修饰符
.left
.right
.middle

### npm
npm i --legacy-peer-deps

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
有时候，我们希望能保持被动态加载组件的状态，已避免反复重复渲染导致的性能问题。为了能实现保持组件状态的功能，我们可以用一个 <keep-alive> 的元素将其动态组件包裹起来。
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


### 组件v-model双向绑定
默认情况下，组件上的 v-model 使用 modelValue 作为 prop 和 update:modelValue 作为事件。我们可以通过向 v-model 传递参数来修改这些名称：

```html
<my-component v-model:title="bookTitle"></my-component>
```


子组件将需要一个 title prop 并发出 update:title 事件来进行同步：

```javascript
app.component('my-component', {
  props: {
    title: String
  },
  emits: ['update:title'],
  template: `
    <input
      type="text"
      :value="title"
      @input="$emit('update:title', $event.target.value)">
  `
})
```

#### 处理 v-model 修饰符
添加到组件 v-model 的修饰符将通过 **modelModifiers** prop 提供给组件

```html
<my-component v-model.capitalize="myText"></my-component>
```

```javascript
const app = Vue.createApp({
  data() {
    return {
      myText: ''
    }
  }
})

app.component('my-component', {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  emits: ['update:modelValue'],
  methods: {
    emitValue(e) {
      let value = e.target.value
      if (this.modelModifiers.capitalize) {
        value = value.charAt(0).toUpperCase() + value.slice(1)
      }
      this.$emit('update:modelValue', value)
    }
  },
  template: `<input
    type="text"
    :value="modelValue"
    @input="emitValue">`
})

app.mount('#app')
```

对于带参数的 v-model 绑定，生成的 prop **名称将为 arg + "Modifiers"**：
```html
<my-component v-model:description.capitalize="myText"></my-component>
```


```javascript
app.component('my-component', {
  props: ['description', 'descriptionModifiers'],
  emits: ['update:description'],
  template: `
    <input type="text"
      :value="description"
      @input="$emit('update:description', $event.target.value)">
  `,
  created() {
    console.log(this.descriptionModifiers) // { capitalize: true }
  }
})

```


### vue 限制输入数字
```html
<!-- 只能输入数字 -->
   <el-input v-model="model.information.researchCycle" oninput="value=value.replace(/[^\d]/g,'')" /> 
<!-- 只能输入数字和小数 -->
   <el-input v-model="model.information.researchCycle" oninput="value=value.replace(/[^0-9.]/g,'')" /> 
```

#### 问题 只做input处理输入中文被替换后model没有实际更新，这样保存到后台的值不正确

##### 解决方法
在输入框失去焦点的时候，把value值赋值给v-model绑定变量，使两者保持一致

```html
<el-input
                    v-model="model.information.researchCycle"
                    oninput="value=value.replace(/[^\d]/g,'')"
                    maxlength="2"
                    @blur="researchCycleChange"
                  />
```

```javascript
    researchCycleChange(e) {
      this.model.information.researchCycle = e.target.value
    }
```


##### 原因
[v-model获取值与.value取值的区别（v-model原理分析](https://blog.csdn.net/qq_41635167/article/details/85736936)

vue源码中在实现v-model绑定时
```javascript
code = `if($event.target.composing)return;${code}` 
```

即当input事件是由IME （即由输入法触发）构成触发的，会直接return，不再获取值




### 问题 
vue 单页应用程序，路由用的hash模式，第一个页面list 拖到页面最底部，点击跳转到detail页面，此时滚动条也还在页面的最底部

### 重要代码
```javascript
// list.vue
 toDetail(id) {
      this.$router.push({ path: '/detail', query: { id: id }})
    }
```

### 已经使用的解决方法
[Vue路由切换后, 页面滚动位置不变BUG处理](https://blog.csdn.net/wxl1390/article/details/111269153)

这三种方法都尝试了，但无法改变滚动条位置

用的chrome浏览器调试，而且调试中也无法用window.scrollTo(0, 0)来使页面回到顶部
