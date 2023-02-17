## 集成DotnetCore

1. 安装nuget包

```shell
install-package Ocelot
```

2. [添加Ocelot配置]()

3. 注册配置和Ocelot

```c#
builder.ConfigureAppConfiguration((hostingContext, config) =>
{
    config
    .AddJsonFile("ocelot.json") //Ocelot
    .AddEnvironmentVariables();//多种环境
});


builder.ConfigureServices(s =>
{
    s.AddOcelot() //AddOcelot
    .AddPolly();
}).ConfigureLogging((hostingContext, logging) =>
{
    Console.WriteLine(logging);
}).UseIISIntegration()
.Configure(app =>
{
    app.UseOcelot() //UseOcelot
    .Wait();
})
.Build()
.Run();
```

[源码]()