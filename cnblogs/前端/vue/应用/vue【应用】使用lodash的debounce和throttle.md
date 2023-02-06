## 使用lodash的debounce和throttle

以debounce为例

1. 安装

```node
npm i --save lodash.debounce
```

2. 引入

```node
import debounce from 'lodash.debounce'
```

3. 使用

```html
<van-search v-model="searchValue" placeholder="输入姓名或工号" @input='handleInput' />
```

```javascript

    // 1.直接函数中使用debounce
    handleInput: debounce(function (val) {
      console.log(val)
    }, 200)

    
```

```javascript
    handleInput(val) {
      console.log(val)
    }

  created() {
　　 this.handleInput = debounce(this.handleInput, 200) // 搜索框防抖
  }

```

