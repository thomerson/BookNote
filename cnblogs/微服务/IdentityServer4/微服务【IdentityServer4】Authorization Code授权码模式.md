## Authorization Code授权码模式

Authorization Code(授权码)模式比Implicit多了一个授权码的流程，即用户认证成功之后，**授权服务器不会马上返回AccessToken，而是先返回一个code(这里称为授权码)**，然后客户端通过code再去获取Token

![授权码模式](https://mmbiz.qpic.cn/mmbiz_png/qQ1zuvjsChSOjFkeQNMcQARQnz2hokzxZrZbwq56DJz4ib9fLDCKn3kDxvxmEoibaMng8fsZmZpV2rQ9OUb3KmRg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)


### 授权码

```Authorization Code```

授权服务器认证成功之后返回的code，这个code有**时效性**，而且**只能使用一次**

更适合有后台的客户端，比如MVC程序，接收code和通过code获取Token的过程都在客户端后台进行，用户无感知；再加上code(授权码)的时效和次数限制，相对来说，这种模式是比较安全的。




