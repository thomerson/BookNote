## 基本配置

```json
{
  "Routes": [
    {
      "UpstreamPathTemplate": "/OcelotTest1/{url}",
      "UpstreamHttpMethod": [ "Get"],
      "DownstreamPathTemplate": "/api/{url}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "localhost",
          "Port": 8000
        }
      ]
    },
    {
      "UpstreamPathTemplate": "/OcelotTest2/{url}",
      "UpstreamHttpMethod": [ "Get"],
      "DownstreamPathTemplate": "/api/{url}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "localhost",
          "Port": 8001
        }
      ]
    }
  ]
}

```

路由万能模板
```json
{
  "DownstreamPathTemplate": "/",
  "DownstreamScheme": "http", // 指定下游是http还是https；
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





### 配置说明


* ```GlobalConfiguration```

  全局配置，所有路由共用的配置

* ```BaseUrl```

  网关对外暴露的地址

* ```ServiceDiscoveryProvider```

  服务发现的相关配置

  * ```Scheme```
    
    Consul启动的主机

  * ```Host``` 

    Consul启动的主机

  * ```Port```
    
    端口

  * ```Type```

    Consul或者其他服务发现框架

### 路由配置

* ```priority```

  优先级

  默认所有配置的路由优先级的值为0,配置的值越大就越优先匹配

  ```json
  {
    "priority": 0, //默认所有配置的路由优先级的值为0,配置的值越大就越优先匹配，将其他模板优先级设高
  }
  ```

* ```RouteIsCaseSensitive```

  路由大小写

```json
{
    "RouteIsCaseSensitive": true, //默认情况下，匹配路由是不区分大小写的，开启大小写匹配
}
    
```

* ```Aggregates```

  路由聚合

```json
{
    "Aggregates":""
}

```

* ```QoSOptions```

  Quality of Service/服务质量

  指一个网络能够利用各种基础技术，为指定的网络通信提供更好的服务能力，是网络的一种安全机制， 是用来解决网络延迟和阻塞等问题的一种技术。

```json

"QoSOptions": {
    "ExceptionsAllowedBeforeBreaking":3,
    "DurationOfBreak":5,
    "TimeoutValue":5000 //请求超过5秒钟，它将自动超时 
}


```
不设置的话，Ocelot默认将所有下游请求的超时时间设置为90秒

*  ```LoadBalancerOptions```

  指定负载均衡算法

