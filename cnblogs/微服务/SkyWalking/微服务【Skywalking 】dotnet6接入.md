## dotnet6接入

1. 安装nuget包

```powershell
install-package SkyAPM.Agent.AspNetCore
```

2. 添加配置

* 在Properties下```launchSettings.json```增加一下配置

ASPNETCORE_HOSTINGSTARTUPASSEMBLIES

SKYWALKING__SERVICENAME

```json
{
  "$schema": "https://json.schemastore.org/launchsettings.json",
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:8084",
      "sslPort": 0
    }
  },
  "profiles": {
    "Demo.SkywalkingAgent": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "launchUrl": "weatherforecast",
      "applicationUrl": "http://localhost:5032",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_HOSTINGSTARTUPASSEMBLIES": "SkyAPM.Agent.AspNetCore", //必须配置
        "SKYWALKING__SERVICENAME": "Service1" // 必须配置，在skywalking做标识，服务名称 
      }
    },
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "launchUrl": "weatherforecast",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}

```

* 不在```launchSettings.json```加，也可以在```Program.cs```加

```c#
Environment.SetEnvironmentVariable("ASPNETCORE_HOSTINGSTARTUPASSEMBLIES", "SkyAPM.Agent.AspNetCore");
Environment.SetEnvironmentVariable("SKYWALKING__SERVICENAME", "Service1");
```

3. 生成```skywalking.json```文件

始终复制

```json
{
  "SkyWalking": {
    "ServiceName": "service1",
    "Namespace": "",
    "HeaderVersions": [
      "sw8"
    ],
    "Sampling": {
      "SamplePer3Secs": -1,
      "Percentage": -1.0
    },
    "Logging": {
      "Level": "Information",
      "FilePath": "logs\\skyapm-{Date}.log"
    },
    "Transport": {
      "Interval": 3000,
      "ProtocolVersion": "v8",
      "QueueSize": 30000,
      "BatchSize": 3000,
      "gRPC": {
        "Servers": "192.168.101.10:11800",
        "Timeout": 10000,
        "ConnectTimeout": 10000,
        "ReportTimeout": 600000,
        "Authentication": ""
      }
    }
  }
}
```


4. 注册skywalking

```c#
builder.Services.AddSkyApmExtensions(); //添加Skywalking相关配置
```

这时候运行项目已经有基本的链路追踪功能

### 自定义链路日志


可以在重要的地方加上，这样就能知道程序跑到这个地方时的关键信息

```c#
using Microsoft.AspNetCore.Mvc;
using SkyApm.Tracing;
using SkyApm.Tracing.Segments;

namespace Demo.SkywalkingAgent.Controllers
{
    public class HomeController : Controller
    {
        private readonly IEntrySegmentContextAccessor _segContext;
        public HomeController(IEntrySegmentContextAccessor segContext)
        {
            _segContext = segContext;
        }

        public IActionResult Index()
        {
            return View();
        }

        /// <summary>
        /// 自定链路日志
        /// </summary>
        /// <returns></returns>
        public string SkywalkingLog()
        {
            //获取全局traceId
            var traceId = _segContext.Context.TraceId;
            _segContext.Context.Span.AddLog(LogEvent.Message("自定义日志1"));
            Thread.Sleep(1000);
            _segContext.Context.Span.AddLog(LogEvent.Message("自定义日志2"));
            return traceId;
        }
       
    }
}

```

### 多服务追踪

例如在service1中调用service2的接口

链路追踪会显示出对应的Service的耗时，还能看到当前服务的详情和打的日志

