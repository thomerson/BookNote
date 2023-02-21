## QoS

```Quality of Service```/服务质量


## Ocelot集成Qos

1. 安装Package

```powershell
install-package Ocelot.Provider.Polly

```

2. AddPolly

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
    .AddPolly(); // Polly
})
```


3. 配置

QoSOptions可以在GlobalConfiguration中配置，也可以在单个route中配置

```json
{
  "GlobalConfiguration": {
    "QoSOptions": {
      "ExceptionsAllowedBeforeBreaking": 3, //发生几次请求异常（比如超时）后进行熔断，该值必须大于0
      "DurationOfBreak": 60, //熔断时间（单位：毫秒），多长时间不能访问
      "TimeoutValue": 5000 //下游请求超时时间（单位：毫秒，默认90秒）
    }
  }
}
```

## 测试

在下游接口添加线程睡眠模拟慢请求，TimeoutValue设置为5000ms,在通过Ocelot访问Product接口会返回```HTTP ERROR 503```

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


