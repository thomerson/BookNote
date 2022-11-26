## 基本配置


## GlobalConfiguration

### BaseUrl



## 优先级

priority

默认所有配置的路由优先级的值为0,配置的值越大就越优先匹配

### 万能模板

```json
{
  "DownstreamPathTemplate": "/",
  "DownstreamScheme": "http",
  "DownstreamHostAndPorts": [
    {
      "Host": "localhost",
      "Port": 36111
    }
  ],
  "priority": 0, //默认所有配置的路由优先级的值为0,配置的值越大就越优先匹配，将其他模板优先级设高
  "UpstreamPathTemplate": "/",
  "UpstreamHttpMethod": [ "Get" ]
} //万能模板，即所有请求都会匹配到该路由模板，但其优先级为最低，如果能匹配到其他模板，优先走其他路由

```

## 路由大小写

RouteIsCaseSensitive

```json
{
    "RouteIsCaseSensitive": true, //默认情况下，匹配路由是不区分大小写的，开启大小写匹配
}
    
```

## 路由聚合


```json
{
    "ReRoutes":[],
    "Aggregates":""
}

```


## QoS

Quality of Service/服务质量

指一个网络能够利用各种基础技术，为指定的网络通信提供更好的服务能力，是网络的一种安全机制， 是用来解决网络延迟和阻塞等问题的一种技术。





https://www.cnblogs.com/snaildev/articles/9151738.html

https://blog.csdn.net/WuLex/article/details/88076814




https://www.cnblogs.com/yilezhu/p/9664977.html


https://www.5axxw.com/questions/content/06b0b2



http://www.yiidian.com/questions/50255



https://blog.csdn.net/qq_36819973/article/details/108727150



https://www.cnblogs.com/cwsheng/p/13458472.html


https://mp.weixin.qq.com/s?__biz=MzU1MzYwMjQ5MQ==&mid=2247484952&idx=1&sn=108c5cb3cadccba57d4acca0fac15da9&chksm=fbf11acccc8693da0d3fab3ac17362dfebf4eb1b8ab5e675b9d075784033f95053f586125c1d#rd



https://www.zhihu.com/people/zong-yi-quan-40


https://zhuanlan.zhihu.com/p/336714163


https://www.zhihu.com/column/c_1314149144102301696



https://zhuanlan.zhihu.com/p/368691424


https://www.cnblogs.com/CoolYYD/



https://www.cnblogs.com/CoolYYD/p/13641361.html



https://www.cnblogs.com/yilezhu/p/9563188.html


https://www.cnblogs.com/yilezhu/p/9557375.html


https://www.cnblogs.com/Nine4Cool/p/13810326.html


https://github.com/catcherwong-archive/APIGatewayDemo

https://www.cnblogs.com/wzk153/p/13935879.html



https://www.cnblogs.com/wzk153/category/1876529.html


https://www.cnblogs.com/wzk153/p/13953084.html






https://www.cnblogs.com/jackcao/p/9937213.html


https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg

