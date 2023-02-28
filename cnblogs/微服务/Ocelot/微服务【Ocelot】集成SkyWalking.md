## SkyWalking

链路追踪

1. 安装nuget包

```powershell
install-package SkyAPM.Agent.AspNetCore
```

2. 添加skywalking配置，生成```skywalking.json```文件

    参考[微服务【Skywalking 】dotnet6接入](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/SkyWalking/微服务【Skywalking%20】dotnet6接入.md)


3. 注册skywalking服务

```c#

builder.Services.AddSkyApmExtensions(); // 添加Skywalking相关配置

builder.Services.AddOcelot(); //ocelot
var app = builder.Build();

```

启动可以在Skywalking UI看到接口调用的追踪信息

