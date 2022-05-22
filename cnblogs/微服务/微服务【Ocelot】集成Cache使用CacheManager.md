## CacheManager

CacheManager是**开源的.Net缓存管理框架**

它不是具体的缓存实现，而是在缓存之上，方便开发人员配置和管理各种不同的缓存，为上层应用程序提供统一的缓存接口的中间层

CacheManager除了缓存管理外，还封装了很多功能，如事件、性能计数器、并发更新等，让开发人员更容易处理和配置缓存



## Ocelot集成CacheManager

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

这个和使用自带的Cache配置一样

## 测试

同样在下游接口添加线程睡眠模拟慢请求，第一次请求时需要10s，第二次请求时瞬间就能获取到结果

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
