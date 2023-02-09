## 坑目录

* [```type="number"```后设置maxlength无效](#typenumber后设置maxlength无效)

* [input ```type="number"```去掉上下箭头样式](#input-typenumber去掉上下箭头样式)


## ```type="number"```后设置maxlength无效

解决方法：

通过自定义指令判断number

```javascript

Vue.directive('numberOnly', {
  bind: function(el) {  
    el.handler = function() {
      el.childNodes[1].value = el.childNodes[1].value.replace(/\D+/g, '');
    }
    el.addEventListener('keyup', el.handler); 
  },
  unbind: function(el) {
    el.removeEventListener('keyup', el.handler);
  }
})
```

```html
<el-input v-model="dataCount" v-number-only maxlength="11"></el-input>
```

## input ```type="number"```去掉上下箭头样式

加上样式

```css
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
    }
    input[type="number"]{
        -moz-appearance: none;
    }
```

**注意**

这个样式放在```<style scoped>```scoped中无效，需要直接放在```<style>```中

