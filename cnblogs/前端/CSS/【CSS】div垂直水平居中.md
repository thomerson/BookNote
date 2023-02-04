## div 垂直水平居中


```html
<!-- HTML -->
<div class="parent">
  <div class="child"></div>
</div>
```

1. 绝对定位

```css
.parent{
  position:relative;
}


/* 第一种 */
.child{
  position:absolute;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%);
}

/* 第二种（需要固定宽高） */
.child{
  position:absolute;
  width:100px;
  height:100px;
  top:50%;
  left:50%;
  margin-left:-50px;
  margin-top:-50px;
}

/* 第三种（需要固定宽高） */
.child{
    position:absolute;
  width:100px;
  height:100px;
  left:0;
  top:0;
  bottom:0;
  right:0;
  margin:auto;
}

/* 第四种（固定宽高 + transform变形） */
.child{
  position: absolute;
  width: 100px;
  height: 100px;
  left: 50%;
  top: 50%;
  background: yellow;
  transform: translate(-50px, -50px);
}

```


2. flex布局

```css

/* 父元素 flex布局，设置居中 */
.parent {
  width: 500px;
  height: 500px;
  background: pink;

  display: flex;
  align-items: center;
  justify-content: center;
}

/* 子元素可以固定宽高 */
.parent. child {
  width: 100px;
  height: 100px;
  background: yellow;
}

/* 子元素不设置宽高 */
.parent. child {
  background: yellow;
}

```

3. table布局

```css
.parent {
  width: 500px;
  height: 500px;
   display: table-cell; /*table布局 */
  text-align: center;
  vertical-align: middle;
  background: pink;
}

.parent .child {
  display: inline-table;
  /* 
  // 这种写法也可以
  display: inline-block; 
  display: inline; */
  width: 100px;
  height: 100px;
  background: yellow;
}
```

4. Grid布局

```css
.parent {
  width: 500px;
  height: 500px;
  display: grid; /* grid布局 */
  background: pink;
}
.parent .child {
  align-self: center;
  justify-self: center;
  width: 100px;
  height: 100px;
  background: yellow;
}
```

## 让一个div垂直水平居中有两种情况

1. 第一种是元素不固定宽高的，有三种方式

    * 使用绝对定位加transform偏移
    * 使用flex布局，设置align-items:center;justify-content:center;
    * 使用grid布局，设置align-self:center; justify-self:center;

2. 如果元素有固定宽高，可以使用：

    * 绝对定位+transform偏移
    * 绝对定位+负margin值
    * 绝对定位+margin:auto
    * 不固定宽高的三种方式

