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
