## 负载均衡

Ocelot在配置路由规则时，开启负载均衡功能，并指定对应的均衡算法，从而实现请求按算法转发到后台服务

```json
{
  "Routes": [
    {
      "DownstreamPathTemplate": "/api/Orders",
      "DownstreamScheme": "http",
      "UpstreamPathTemplate": "/ocelot/order",
      "UpstreamHttpMethod": [ "Get" ],
      "LoadBalancerOptions": {
        "Type": "LeastConnection" // 负载均衡算法
      },
      "ServiceName": "OrderService", // 没有DownstreamHostAndPorts，表示下游服务，不是由Ocelot直接指定，而是通过Consul查询得到
      "Priority": 2
    }]
}

```

### 负载均衡算法

```LoadBalancerOptions``` 支持的相关负载均衡算法


* ```LeastConnection``` 把新请求转发到请求最少的后台服务上；

    最少连接，跟踪哪些服务正在处理请求，并把新请求发送到现有请求最少的服务上。该算法状态不在整个Ocelot集群中分布


* ```RoundRobin``` 

    将请求轮询轮询转发都配置的后台服务上；

* ```NoLoadBalancer``` 不负载均衡；

* ```CookieStickySessions``` 

    使用cookie关联相关的请求到指定的服务；

    **粘性会话**

    ```json
    "LoadBalancerOptions": {
        "Type": "CookieStickySessions",
        "Key": "ASP.NET_SessionId",
        "Expiry": 1800000
    },
    ```

    * ```Key```是您希望用于粘性会话的cookie的名称

    * ```Expiry```是希望会话被粘合的时间，以毫秒为单位

