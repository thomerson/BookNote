## Consul的api文档

[api-docs](https://developer.hashicorp.com/consul/api-docs)

## 集成DotnetCore

1. Nuget安装包

```install-package Consul```

可以参考```api-docs```


2. 添加Consul中间件

```c#

using Consul;
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using System;

namespace Demo.ConsulCenter.Midware
{
    public static class ConsulMidware
    {
        /// <summary>
        /// 服务注册到consul
        /// </summary>
        /// <param name="app"></param>
        /// <param name="lifetime"></param>
        public static IApplicationBuilder RegisterConsul(this IApplicationBuilder app, IConfiguration configuration, IHostApplicationLifetime lifetime)
        {
            var consulClient = new ConsulClient(c =>
            {
                //consul地址
                c.Address = new Uri(configuration["ConsulSetting:ConsulAddress"]);
            });

            var registration = new AgentServiceRegistration()
            {
                ID = Guid.NewGuid().ToString(),//服务实例唯一标识
                Name = configuration["ConsulSetting:ServiceName"],//服务名
                Address = configuration["ConsulSetting:ServiceIP"], //服务IP
                Port = int.Parse(configuration["ConsulSetting:ServicePort"]),//服务端口 因为要运行多个实例，端口不能在appsettings.json里配置，在docker容器运行时传入
                Check = new AgentServiceCheck()
                {
                    DeregisterCriticalServiceAfter = TimeSpan.FromSeconds(5),//服务启动多久后注册
                    Interval = TimeSpan.FromSeconds(10),//健康检查时间间隔
                    HTTP = $"http://{configuration["ConsulSetting:ServiceIP"]}:{configuration["ConsulSetting:ServicePort"]}{configuration["ConsulSetting:ServiceHealthCheck"]}",//健康检查地址
                    Timeout = TimeSpan.FromSeconds(5)//超时时间
                }
            };

            //服务注册
            consulClient.Agent.ServiceRegister(registration).Wait();

            //应用程序终止时，取消注册
            lifetime.ApplicationStopping.Register(() =>
            {
                consulClient.Agent.ServiceDeregister(registration.ID).Wait();
            });


            return app;
        }
    }
}
```

在```Configure```中注册使用

```c#

//服务注册
app.RegisterConsul(Configuration, lifetime);

```


3. 添加consul配置

对应上步骤的```ConsulSetting```配置项


```json
  "ConsulSetting": {
    "ServiceName": "OrderService",
    "ServiceIP": "localhost",
    "ServicePort": 49725,
    "ServiceHealthCheck": "/HealthCheck",
    "ConsulAddress": "http://localhost:8500/" //Consul地址
  }
```


4. 添加健康检测的接口

对应上步骤的```ServiceHealthCheck```配置

```c#

using Microsoft.AspNetCore.Mvc;

namespace Demo.ConsulCenter.Controller
{
    [Route("[controller]")]
    [ApiController]
    public class HealthCheckController : ControllerBase
    {
        /// <summary>
        /// 健康检查接口
        /// </summary>
        /// <returns></returns>
        [HttpGet]
        public IActionResult Get()
        {
            return Ok();
        }
    }
}

```

[源码](https://github.com/thomerson/Demo/tree/main/DotnetCore/Demo.Consul)
