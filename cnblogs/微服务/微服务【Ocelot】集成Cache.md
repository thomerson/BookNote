
## Ocelot集成Cache

在需要cache的route上直接加上配置```FileCacheOptions```就可以实现cache

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


