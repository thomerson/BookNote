## 限流

1. 修改Route节点中的添加如下节点

也可以放在```GlobalConfiguration```中，所有下游服务都执行限流配置

配置策略：

客户端在10分钟之内只允许请求一次，在请求之后3秒钟之后可以重试

```json
 "RateLimitOptions": {  //限流配置
        "ClientWhitelist": [],  //白名单，也就是不受限流控制的客户端
        "EnableRateLimiting": true, //是否开启限流
        "Period": "10m",  //在一段时间内允许的请求次数
        "PeriodTimespan": 3, //客户端的重试间隔数，也就是客户端间隔多长时间可以重试
        "Limit": 1, //在一段时间内允许的请求次数
        "QuotaExceededMessage": "您的请求量超过了配额1/10分钟", //限流以后的提示信息
        "HttpStatusCode": 999 //超出配额时，返回的http状态码
}
```

