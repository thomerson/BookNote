## compression-webpack-plugin

通过gzip压缩算法，将前端打包好的资源文件进一步压缩，生成指定的、体积更小的压缩文件，让浏览器能够更快的加载资源

1. 安装

```javascript
npm install compression-webpack-plugin --save-dev
```

2. 使用

```javascript
const CompressionPlugin = require("compression-webpack-plugin");

module.exports = {
  plugins: [new CompressionPlugin()],
};
```

3. 修改```vue.config.js```配置文件

```javascript
let productionGzipExtensions = /\.(js|css|json|txt|html|ico|svg)(\?.*)?$/i;

module.exports = {
  chainWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
    config.plugin("CompressionPlugin").use('compression-webpack-plugin', [{
          filename: '[path][base].gz',
          algorithm: 'gzip',
          // 要压缩的文件（正则）
          test: productionGzipExtensions,
          // 最小文件开启压缩
          threshold: 10240,
          minRatio: 0.8
        }])
    }
  },
}

```

打包到Nginx部署

4. Nginx设置

```nginx

gzip_static on;
gzip_proxied expired no-cache no-store private auth;
gzip_disable "MSIE [1-6]\.";

```