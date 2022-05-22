
## Ocelot集成Redis作为Cache

使用CacheManager作为缓存管理框架，底层实现用Redis

1. 安装Package

```
install-package Ocelot.Cache.CacheManager
install-package CacheManager.StackExchange.Redis
install-package CacheManager.Serialization.Json 


```

2. 注册service

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
    .AddPolly()
    .AddCacheManager(x =>
    {
        x.WithRedisConfiguration("redis", config =>
        {
            config.WithAllowAdmin().WithPassword("redisPsd")//Redis 配置 密码
            .WithDatabase(0)  //数据库
            .WithEndpoint("localhost",6379); //Redis 服务地址 端口


        })
        .WithRedisCacheHandle("redis",true);  //using CacheManager.Core;

        x.WithJsonSerializer(); //数据序列化方式

        //x.WithDictionaryHandle(); //ocelot的CacheManager  字典方式处理
    });

    //s.AddSingleton<IOcelotCache<CachedResponse>, OcelotCache<CachedResponse>>();
    //.AddKubernetes();
    //.AddSingletonDefinedAggregator<FooAggregator>();
    //.AddConsul()//Consul
    //.AddConfigStoredInConsul();  // ConsulConfig
})

```


3. 配置

配置后看到


