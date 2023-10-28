## fiddle

Fiddler官网：```https://www.telerik.com/download/fiddler``` 

可以安装使用```fiddler-classic```

1. Fiddle配置

电脑打开fiddler，手机电脑同一网段。fiddler打开https抓包(Tools->Options->HTTPS)，
同时配置好端口(Tools->Options->Connections)

![fiddle-options-https](https://upload-images.jianshu.io/upload_images/7669036-b64012e7995760c4.png?imageMogr2/auto-orient/strip|imageView2/2/w/569/format/webp)

端口配置

![fiddle-options-connections](https://upload-images.jianshu.io/upload_images/7669036-015a0cc0aa8d54d4.png?imageMogr2/auto-orient/strip|imageView2/2/w/562/format/webp)

2. 查看电脑ip

```ipconfig```命令

![ipconfig](https://upload-images.jianshu.io/upload_images/7669036-22a8831abd654c9f.png?imageMogr2/auto-orient/strip|imageView2/2/w/653/format/webp)

3. 手机代理配置

打开手机，连接和电脑同一网络的WiFi，找到高级设置或者代理设置，代理选择手动，然后主机名输入上一步拿到的电脑的ipv4地址，端口输入fiddler配置的监听端口

![agent](https://upload-images.jianshu.io/upload_images/7669036-43759650d996d65d.png?imageMogr2/auto-orient/strip|imageView2/2/w/470/format/webp)

4. 安装fiddle证书

手机打开浏览器，访问刚刚输入的```ip:port```,找到蓝色的下载链接，点击下载

![fiddle-certificate](https://upload-images.jianshu.io/upload_images/7669036-4e5799f50e1deb00.png?imageMogr2/auto-orient/strip|imageView2/2/w/696/format/webp)

设置安装CA证书

![CA-certificate](https://upload-images.jianshu.io/upload_images/7669036-ef3389a39dafc6bf.png?imageMogr2/auto-orient/strip|imageView2/2/w/339/format/webp)

5. 打开app进行抓包

首先需要关闭左小角的capture，避免抓到电脑的包。然后手机上执行对应的操作就可以看到抓到的请求数据了

![fiddle-catch](https://upload-images.jianshu.io/upload_images/7669036-b946b27aeeb79537.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

