## element-ui单选框

```html
<el-radio label="1">药物涂层支架</el-radio>
```

如果需要绑定**数值**或者**布尔类型**的值，需要在label前加上: ⭐

```html
<el-radio :label="true">是</el-radio>
```

### radio绑定model的特殊写法
```html
<input type="radio" v-model="pick" v-bind:value="a" />
```