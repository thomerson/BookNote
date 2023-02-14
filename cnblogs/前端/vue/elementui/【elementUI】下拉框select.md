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
