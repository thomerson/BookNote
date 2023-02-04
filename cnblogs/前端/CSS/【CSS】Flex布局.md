## Flex布局

```Flexible Box```/弹性布局

用来为盒状模型提供最大的灵活性

```display: flex;```

⭐注意，设为Flex布局以后，子元素的float、clear和vertical-align属性将失效

### 容器属性

* ```flex-direction```

    决定主轴的方向（即项目的排列方向）

* ```flex-wrap```

    换行

    
* ```flex-flow```

    是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap


* ```justify-content```

    在主轴上的对齐方式

    ```css
    .box {
        justify-content: flex-start | flex-end | center | space-between | space-around;
    }
    ```

* ```align-items```

    在交叉轴上如何对齐

    ```css
    .box {
        align-items: flex-start | flex-end | center | baseline | stretch;
    }
    ```

* ```align-content```

    多根轴线的对齐方式


### 项目属性

* order

    定义项目的排列顺序。数值越小，排列越靠前，默认为0。

* flex-grow

    定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

* flex-shrink

    定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小

* flex-basis

    定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

    ```css
    .item {
        flex-basis: <length> | auto; /* default auto */
    }
    ```

    设为跟width或height属性一样的值（比如350px），则项目将占据固定空间


* flex

    是flex-grow, flex-shrink 和 flex-basis的简写，默认值为```0 1 auto```。后两个属性可选
* align-self

    允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch

