## 坑目录

* [```type="number"```后设置maxlength无效](#typenumber后设置maxlength无效)

* [input ```type="number"```去掉上下箭头样式](#input-typenumber去掉上下箭头样式)

* [vue 限制输入数字](#vue-限制输入数字)


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
