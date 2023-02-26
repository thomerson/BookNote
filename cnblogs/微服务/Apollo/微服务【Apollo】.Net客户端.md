## .Net客户端

1. 安装nuget包

```powershell
install-package Com.Ctrip.Framework.Apollo.Configuration
```

2. 添加Apollo配置

```json

// Apollo默认有一个“SampleApp”应用，“DEV”环境 和 “timeout” KEY

"apollo": {
"AppId": "SampleApp",
"Env": "DEV",
"MetaServer": "http://192.168.2.133:8080"
}
```

3. 修改program

```c#

// 添加Apollo配置中心
IConfiguration configuration = new ConfigurationBuilder()
.AddJsonFile("appsettings.json")
.Build();

builder.Host.ConfigureAppConfiguration((context, b) =>
{
    b.AddApollo(configuration.GetSection("apollo"))
    .AddDefault();
});


// 或者

builder.Host.ConfigureAppConfiguration((context, b) =>
{
    b.AddApollo(b.Build().GetSection("apollo"))
     .AddDefault();
});
```

4. 修改control，读取apollo配置

```c#

public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    IConfiguration _config;

    public HomeController(ILogger<HomeController> logger, IConfiguration config)
    {
        _logger = logger;
        _config = config;
    }

    public IActionResult Index()
    {
        var data = _config["Key"];  // 从apollo读取配置
        return View();
    }
}

```

### 参考

* [Apollo.ConfigurationManager](https://github.com/apolloconfig/apollo.net/blob/main/src/Apollo.ConfigurationManager/README.md)


