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


