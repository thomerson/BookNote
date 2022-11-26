## 安装包

```
Install-Package IdentityServer4

```

## IdentityServer配置

* API: Authorization Server 保护了哪些 API （资源）；
* Client: 哪些客户端 Client（应用） 可以使用这个 Authorization Server；
* Users: 指定可以使用 Authorization Server 授权的 Users（用户）

```c#
public static class IdentityServerConfig
{
    //public static IEnumerable<IdentityResource> GetIdentityResourceResources()
    //{
    //    return new List<IdentityResource>
    //    {
    //        //必须要添加，否则报无效的scope错误
    //        new IdentityResources.OpenId(),
    //        new IdentityResources.Profile()
    //    };
    //}

    /// <summary>
    /// api资源列表
    /// </summary>
    public static IEnumerable<ApiResource> Apis => new List<ApiResource> {
        new ApiResource("IDS4.api")
    };

    public static IEnumerable<Client> Clients => new List<Client> {
        new Client(){
            ClientId =  "ClientA",
            AllowedGrantTypes = GrantTypes.ClientCredentials,
            AllowedScopes = new List<string> { "IDS4.api" },
            ClientSecrets  = { new Secret("secret".Sha256())}
        }
    };

    /// <summary>
    ///  指定可以使用 Authorization Server 授权的 Users（用户）
    /// </summary>
    /// <returns></returns>
    public static IEnumerable<User> Users() => new List<User>() {
    new User(){UserId="admin",Name="admin" },
    new User(){UserId="guest",Name="guest" }
    };
}

```

## 注册中间件

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddIdentityServer()
        .AddDeveloperSigningCredential()
        .AddInMemoryApiResources(IdentityServerConfig.Apis)
        .AddInMemoryClients(IdentityServerConfig.Clients);
}

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseRouting();

    app.UseIdentityServer();

    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

启动IdentityServer访问```/.well-known/openid-configuration```，可以看到IdentityServer的各种元数据信息

首次启动时，IdentityServer将会创建一个开发人员签名密钥，它是一个名为tempkey.rsa的文件。



## 测试

通过一个client来调用一个需要Authentication的api，看看权限访问是否启用

### API
1. 新建一个API网站，安装包

```
Install-Package IdentityServer4.AccessTokenValidation
```

2. 中间件启用Authentication

```C#

public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddAuthorization();

    services.AddAuthentication("Bearer") //AddAuthentication
    .AddJwtBearer("Bearer", options =>
    {
        options.Authority = "http://localhost:5000";
        options.RequireHttpsMetadata = false;

        options.Audience = "api1";
    });
}

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseRouting();

    app.UseAuthentication();

    app.UseAuthorization(); //UseAuthorization

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}

```

3. 添加一个控制器，加上```Authorize```特性，启用授权访问

```c#
[Route("identity")]
[Authorize]
public class IdentityController : ControllerBase
{
    [HttpGet]
    public JsonResult Index()
    {
        var result = from c in User.Claims select new { c.Type, c.Value };
        return new JsonResult(result);
    }
}

```

此时访问这个接口返回```401```

### Client

通过Client调用IdentityServer获取token，然后同请求时Header添加token后再调用API


1. 新建Client控制台程序，安装```IdentityModel```包

```
Install-Package IdentityModel
```




https://blog.csdn.net/weixin_39305029/article/details/123497673

https://github.com/IdentityServer/IdentityServer4

https://github.com/zyq025/IDS4Demo