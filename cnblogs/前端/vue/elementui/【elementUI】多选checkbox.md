## element-ui checkbox 0/1绑值

v-model加载时无法直接选中，需要用checked去帮助默认选中

```html
 <el-checkbox v-model="scope.row.noInspectionRecord" true-label="1" false-label="0" :checked="scope.row.noInspectionRecord==1" />
```


## element-ui checkboxgroup 不支持对象数组

不用checkboxgroup 用v-for 一个一个去绑定

