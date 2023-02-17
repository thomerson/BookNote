## Ocelot请求聚合


可以把多个正常的Routes打包并映射到一个对象来对客户端的请求进行响应


在ocelot.json中进行配置

1. 为每个Route设置一个Key属性：如："Key": "Catalog"

2. 配置Aggregates节点，并指定RouteKeys中指定1中设置的Key值组成的数组，并设置UpstreamPathTemplate匹配上游用户请求

```json
{
  "Aggregates": [
    {
      "RouteKeys": [ //Keys
        "Catalog",
        "Ordering"
      ],
      "UpstreamPathTemplate": "/GetOrderDetail/{id}"
    }
  ],
  "GlobalConfiguration": {

  },
  "Routes": [
    {
      "DownstreamPathTemplate": "/api/Values/{id}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "localhost",
          "Port": 5332
        }
      ],
      "UpstreamPathTemplate": "/Catalog/{id}",
      "UpstreamHttpMethod": [ "Get", "Post" ],
      "LoadBalancerOptions": {
        "Type": "RoundRobin"
      },
      "Key": "Catalog" //Key
    },
    {
      "DownstreamPathTemplate": "/api/Values/{id}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "localhost",
          "Port": 5331
        }
      ],
      "UpstreamPathTemplate": "/Ordering/{id}",
      "UpstreamHttpMethod": [ "Get", "Post" ],
      "LoadBalancerOptions": {
        "Type": "RoundRobin"
      },
      "Key": "Ordering"//Key
    }
  ]
}

```

3. 返回结果为设置的Key组合的数据结构

```json
{"Catalog":{},"Ordering":{}}
```

4. 请求异常

    * 某个服务异常，只是该请求返回错误信息

    * 某个服务宕机，整个返回502错误

5. [自定义聚合](#自定义聚合)

    


### 自定义聚合

实现```IDefinedAggregator```接口


1. 定义自定义聚合器

```c#

public class FakeDefinedAggregator : IDefinedAggregator
    {
        public FakeDefinedAggregator()
        {
        }
        public async Task<DownstreamResponse> Aggregate(List<HttpContext> responses)
        {

            List<string> result = new List<string>();
            foreach (var item in responses)
            {
                byte[] tmp = new byte[item.Response.Body.Length];
                await item.Response.Body.ReadAsync(tmp, 0, tmp.Length);
                var val = Encoding.UTF8.GetString(tmp);
                result.Add(val);
            }
            var merge = string.Join(";", result.ToArray());
            List<Header> headers = new List<Header>();
            return new DownstreamResponse(new StringContent(merge), HttpStatusCode.OK, headers, "some reason");
        }
    }

```

2. 注册自定义聚合器

```c#
services.AddOcelot()
        .AddSingletonDefinedAggregator<FakeDefinedAggregator>();
```

3. 修改Ocelot配置文件

```json

"Aggregates": [
    {
      "ReRouteKeys": [
        "Orders",
        "Products"
      ],
      "UpstreamPathTemplate": "/GetOrderDetail/{id}",
      "Aggregator": "FakeDefinedAggregator"
    }
  ]
  
```