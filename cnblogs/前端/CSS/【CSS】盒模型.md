## 盒模型

![盒模型](https://www.runoob.com/images/box-model.gif)

* margin 不参与计算盒子的大小

* border

* padding

* content

## 块级盒子与内联盒子

设置盒子display属性

* 块级盒子
    * 盒子会换行显示
    * width和height属性可以发挥作用
    * 内边距padding外边距margin边框border会将其他元素从当前盒子周围推开
    * 默认情况下盒子宽度会跟父容器一样宽


* 内联盒子

    * 盒子不会换行
    * width和height属性不起作用
    * 垂直方向的内边距、外边距以及边框会被应用，但是不会把其它处于inline状态的盒子推开。
    * 水平方向的内边距、外边距以及边框会被应用，并且会把其它处于inline状态的盒子推开




## IE盒模型和标准盒模型

修改盒模型为IE盒模型

```css
box-sizing:border-box;
```

* IE盒模型/怪异盒模型

    * 盒子的**宽度**是我们定义的**width**值

    <br>IE6、IE7、IE8下默认使用的是IE盒模型
    <br>

* 标准盒模型（默认）

    * 盒子的宽度是内容的width值加上左右内边距加上左右边框的值