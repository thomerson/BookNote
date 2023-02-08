## Webpack

前端资源**加载**/**打包**工具

根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

依赖node环境

1. 安装

```node
cnpm install webpack -g
```

2. 打包命令

```node
webpack runoob1.js bundle.js
```


### LOADER

Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换


添加 css 文件，就需要使用到 ```css-loader``` 和 ```style-loader```

* ```css-loader``` 会遍历 CSS 文件，然后找到 url() 表达式然后处理他们

* ```style-loader``` 会把原来的 CSS 代码插入页面中的一个 style 标签中


### 配置文件

根目录```webpack.config.js```文件

```javascript
module.exports = {
    entry: "./runoob1.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    }
};
```

### 插件

* ```BannerPlugin```

    用于在文件头部输出一些**注释**信息。

```javascript
var webpack=require('webpack');
 
module.exports = {
    entry: "./runoob1.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    },
    plugins:[
    new webpack.BannerPlugin('菜鸟教程 webpack 实例')
    ]
};
```


### 开发环境监听模式

不想每次修改模块后都重新编译，那么可以启动监听模式。开启监听模式后，没有变化的模块会在编译后缓存到内存中，而不会每次都被重新编译，所以监听模式的整体速度是很快的。

```node
webpack --progress --colors --watch

```
* colors 让编译的输出内容带有进度和颜色

* watch 监听
