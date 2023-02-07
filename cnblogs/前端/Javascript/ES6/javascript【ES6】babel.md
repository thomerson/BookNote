## babel

作用：**把ES6编译成ES5**

三个阶段：解析，转换，生成

<!-- TODO -->

### 使用方法

* 使用单体文件 (standalone script)

* 命令行 (cli)

* 构建工具的插件 (webpack 的 babel-loader, rollup 的 rollup-plugin-babel)


只是入口不同而已，调用的 babel 内核，处理方式都是一样的


### babel-cli

cli 就是命令行工具。安装了 babel-cli 就能够在命令行中使用 babel 命令来编译文件。


### babel-node

不用单独安装，而是随babel-cli一起安装。然后，执行babel-node就进入PEPL环境


### babel-register

babel-register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码。


### babel-core

如果某些代码需要调用Babel的API进行转码，就要使用babel-core模块。



### 配置文件.babelrc


Babel的配置文件是.babelrc，存放在项目的根目录下。


该文件用来设置转码规则和插件，基本格式如下

```javascript

{
  "presets": [
      "es2015",
      "react",
      "stage-2"
  ],  //设定转码规则
  "plugins": []
}
```

官方提供以下的规则集，可以根据需要安装

```node
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

# react转码规则
$ npm install --save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```


[https://www.ruanyifeng.com/blog/2016/01/babel.html](https://www.ruanyifeng.com/blog/2016/01/babel.html)