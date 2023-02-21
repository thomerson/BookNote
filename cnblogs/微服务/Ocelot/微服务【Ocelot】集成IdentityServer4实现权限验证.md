## 集成IdentityServer4实现权限验证

[微服务【IdentityServer4】客户端凭证模式]()

在ids4中分三个角色

* ids4 server

* api

* client

### 创建ids4server

具体步骤参考[微服务【IdentityServer4】客户端凭证模式]()


### Ocelot集成ids4


1. 安装nuget包

```powershell
install-package IdentityServer4.AccessTokenValidation
```

2. 注册ids4

```c#
builder.ConfigureServices(s =>
{
    s.AddAuthentication()
    .AddJwtBearer("customer.api", option =>
    {
        option.Audience = "customer.api";
        option.Authority = "http://localhost:5146";
        option.RequireHttpsMetadata = false;
    }).AddJwtBearer("product.api", y =>
    {
        y.Audience = "product.api";
        y.Authority = "http://localhost:5026";
        y.RequireHttpsMetadata = false;
    });
}
```

3. 修改Ocelot配置，添加认证信息

```json
"Routes": [
    {
      "DownstreamPathTemplate": "/api/Customers",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "localhost",
          "Port": 5146
        }
      ],
      "UpstreamPathTemplate": "/customers",
      "UpstreamHttpMethod": [ "Get" ],
      "Key": "Tom",
      "AuthenticationOptions": { //授权信息
        "AuthenticationProviderKey": "customer.api",
        "AllowedScopes": [ "customer.api" ]
      }
      //"ServiceName": "customer"
      //"LoadBalancerOptions": {
      //  "Type": "LeastConnection"
      //}
    },
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
      },
      "AuthenticationOptions": { //授权信息
        "AuthenticationProviderKey": "product.api",
        "AllowedScopes": [ "product.api" ]
      }
    }
  ]

```

4. 请求开始配置的聚合api

返回401

5. 获取token再请求聚合api


<!-- TODO -->
<!-- 
疑问：
https://www.cnblogs.com/cwsheng/p/13418974.html
https://www.cnblogs.com/markjiang7m2/p/10932805.html
Ocelot如何请求ids4的服务的，没有配置ids4的地址的地方

Ocelot client请求ids4拿到这个client对应的resource列表，看serviceKey中有没有对应的resource
 -->


<!-- TODO  源码调试有问题 -->
 [源码](https://github.com/thomerson/Demo/tree/main/dotnet6/DemoOcelot)
