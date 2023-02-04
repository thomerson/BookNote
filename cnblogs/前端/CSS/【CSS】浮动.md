## 浮动

会使元素向左或向右移动，其周围的元素也会重新排列

**飘离标准流，不占位置，会影响标准流**。只能左右浮动

因为浮动元素脱离了普通流，会出现一种高度坍塌的现象：原来的父容器高度是内部元素撑开的，但是当内部元素浮动后，脱离普通流浮动起来，那**父容器的高度就坍塌，变为高度0px**。


## 清除浮动

1. 利用clear样式

    ```css
    clear:both;
    ```

    原理：添加一个空div，利用css提高的clear:both清除浮动，让父级div能自动获取到高度。

    优点：简单，代码少，浏览器支持好，不容易出现怪问题。

    缺点：不少初学者不理解原理；如果页面浮动布局多，就要增加很多空div，让人感觉很不爽。

建议：此方法是以前主要使用的一种清除浮动方法。

2. 父级div定义```overflow:hidden```或者```overflow:auto```

    原理：使用overflow:hidden时，浏览器会自动检查浮动区域的高度。

    优点：简单，代码少，浏览器支持好。

    缺点：必须定义width或zoom:1，不能和position配合使用，因为超出的尺寸的会被隐藏。

    建议：只推荐没有使用position或对overflow:hidden理解的朋友使用。


3. 父级div定义height

    原理：父级div手动定义height，就解决了父级div无法自动获取到高度的问题。

    优点：简单，代码少，容易掌握。

    缺点：只适合高度固定的布局，要给出精确的高度，如果高度和父级div不一样时，会产生问题。

    建议：不推荐使用，只建议高度固定的布局时使用。

4. 父级div定义伪类:after和zoom

    优点：浏览器支持好，不容易出现怪问题（目前：大型网站都有使用，如：腾迅，网易，新浪等等）。

    缺点：代码多，不少初学者不理解原理，要两句代码结合使用，才能让主流浏览器都支持。
    
    建议：推荐使用，建议定义公共类，以减少CSS代码。

    ```html
    <div class="div1 clearfloat">
        <div class="left">Left</div>
        <div class="right">Right</div>
    </div>

    <style type="text/css">　
    .div1{
        background:#000080;
        border:1px solid red;
        }
    
    .left{
        float:left;
        width:20%;
        height:200px;
        background:#DDD
        }　  
    
    .right{
        float:right;
        width:30%;
        height:80px;
        background:#DDD
        }  　  
    
    .clearfloat:after{
        display:block;
        clear:both;
        content:"";
        visibility:hidden;
        height:0
        }　  
    
    .clearfloat{
        zoom:1
        }
    </style>
    ```
