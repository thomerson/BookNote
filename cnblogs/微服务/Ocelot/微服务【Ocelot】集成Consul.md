## 集成Consul


1. 安装包

```powershell
Install-Package Ocelot.Provider.Consul
```

2. 注册Consul

```c#

builder.ConfigureServices(s =>
{
    s.AddOcelot() 
    .AddConsul();   // AddConsul
}).ConfigureLogging((hostingContext, logging) =>
{
    Console.WriteLine(logging);
}).UseIISIntegration()
.Configure(app =>
{
    app.UseOcelot() 
    .Wait();
})
.Build()
.Run();
```

3. 在Ocelot配置中添加Consul相关配置


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
    }
  ]
}

```

在ocelot请求```/ocelot/order```,会通过consul发现到OrderService的地址请求```/api/Orders```路由

[源码](https://github.com/thomerson/Demo/tree/main/dotnet6/DemoOcelot)

[Consul源码](https://github.com/thomerson/Demo/tree/main/DotnetCore/Demo.Consul)
