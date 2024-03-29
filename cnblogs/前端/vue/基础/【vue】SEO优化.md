## SEO

```Search engine optimization```/搜索引擎优化

了提升网页在搜索引擎自然搜索结果中（非商业性推广结果）的收录数量以及排序位置而做的优化行为，是为了从搜索引擎中获得更多的免费流量，以及更好的展现形象。

### vue需要做SEO原因

单页面SPA应用就是**实时渲染**的，爬虫爬不到

## 解决方案

1. SSR

```Server-Side Rendering```/服务端渲染

将组件或页面通过服务器生成html，再返回给浏览器，如```nuxt.js```

2. 静态化

    有两种方式

    * 通过程序将动态页面抓取并保存为静态页面，这样的页面的实际存在于服务器的硬盘中

    * 通过WEB服务器的 ```URL Rewrite```的方式，它的原理是通过web服务器内部模块按一定规则将外部的URL请求转化为内部的文件地址，一句话来说就是把外部请求的静态地址转化为实际的动态页面地址，而静态页面实际是不存在的

3. 预渲染

    原理是通过Nginx配置，判断访问来源是否为爬虫，如果是则搜索引擎的爬虫请求会转发到一个node server，再通过PhantomJS等来解析完整的HTML，返回给爬虫

    * 使用```Phantomjs```针对爬虫处理

    ![Phantomjs](https://static.vue-js.com/25be6630-3ac7-11eb-ab90-d9ae814b240d.png)

    
    * [prerender-spa-plugin](https://juejin.cn/post/7059771777525743624)
