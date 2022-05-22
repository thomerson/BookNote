
## Ocelot集成Cache

1. 安装Package

```
install-package Ocelot.Cache.CacheManager

```

2. 使用CacheManager

```c#
var builder = new WebHostBuilder();

builder.ConfigureAppConfiguration((hostingContext, config) =>
{
    config
    .SetBasePath(hostingContext.HostingEnvironment.ContentRootPath)
    .AddJsonFile("appsettings.json", true, true)
    .AddJsonFile($"appsettings.{hostingContext.HostingEnvironment.EnvironmentName}.json", true, true) //reloadOnChange:更改时重新加载 JSON 配置
    .AddJsonFile("ocelot.json")
    .AddEnvironmentVariables();//多种环境
});


builder.ConfigureServices(s =>
{
    s.AddOcelot()
    // .AddPolly()
    .AddCacheManager(x =>
    {
        x.WithDictionaryHandle();
    });
})
```


3. 配置

在需要cache的route上加上配置```FileCacheOptions```

```json
{
  "DownstreamPathTemplate": "/api/products",
  "DownstreamScheme": "http",
  "DownstreamHostAndPorts": [
    {
      "Host": "localhost",
      "Port": 5026
    }
  ],
  "UpstreamPathTemplate": "/products",
  "UpstreamHttpMethod": [ "Get" ],
  "Key": "Jerry",
  "FileCacheOptions": {
    "TtlSeconds": 15,
    "Region": "myRegion"
  }
}
```

* TtlSeconds
  配置有效期的时间，单位为秒

* Region
  区域名，即分区缓存数据；Oeclot可以提供缓存管理接口，然后指定区域清除缓存

## 测试

在下游接口添加线程睡眠模拟慢请求，第一次请求时需要10s，第二次请求时瞬间就能获取到结果

```c#
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public JsonResult Get()
    {
        Thread.Sleep(10000);
        return new JsonResult(new List<string>() { "Fish", "Milk" });
    }
}
```


