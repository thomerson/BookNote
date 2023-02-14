## 父组件向子组件传值

* props

* 依赖注入provide/inject



## 子组件向父组件传值

* emit

* 作用域slot


## 双向

* vuex/pinia状态管理


## props

**单向下行绑定**,**不应该在一个子组件内部改变 prop**,需要修改的话在data中定义一个对象，并将prop作为初始值


```html
<!-- 父组件 -->
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


## emit

```html
<!-- 父组件 -->
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

