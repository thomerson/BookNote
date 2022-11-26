## Client Credentials/客户端凭证模式

1. 客户端提供证明，访问ids4服务器，获取token

2. ids4验证客户端，通过返回token 

3. 客户端使用token访问api

4. api验证token，验证通过返回数据

## 实现

### IdentityServer

1. 添加```Config.cs```，配置```client```和```scope```

```c#
public static IEnumerable<ApiScope> Scopes => new List<ApiScope> {
    new ApiScope("api1"){ },
    //new ApiScope("shop.admin")
};


public static IEnumerable<Client> Clients => new List<Client> {
    new Client(){
        ClientId =  "ClientA",
        AllowedGrantTypes = GrantTypes.ClientCredentials,
        AllowedScopes = new List<string> { "api1" },
        ClientSecrets  = { new Secret("secret".Sha256())}
    }
};

```


2. 添加IdentityServer中间件

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddIdentityServer()
        .AddInMemoryApiScopes(IdentityServerConfig.Scopes)
        .AddInMemoryClients(IdentityServerConfig.Clients)
        
        .AddDeveloperSigningCredential();   //※※※

}


public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseIdentityServer();

    app.UseRouting();

    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}


```

3. 启动IdentityServer访问```/.well-known/openid-configuration```，可以看到IdentityServer的各种元数据信息

首次启动时，IdentityServer将会创建一个开发人员签名密钥，它是一个名为```tempkey.rsa```的文件。




## API

1. 安装包

```
Install-Package  IdentityServer4.AccessTokenValidation
```


2. 添加中间件，**必须在添加 MVC 之前添加**

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddAuthorization();

    services.AddAuthentication("Bearer")
    .AddJwtBearer("Bearer", options =>
    {
        options.Authority = "http://localhost:5600";
        options.RequireHttpsMetadata = false;
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateAudience = false
        };

        //options.Audience = "api1";
    });

    // adds an authorization policy to make sure the token is for scope 'api1'
    services.AddAuthorization(options =>
    {
        options.AddPolicy("ApiScope", policy =>
        {
            policy.RequireAuthenticatedUser();
            policy.RequireClaim("scope", "api1");
        });
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

    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers()
        .RequireAuthorization("ApiScope");
    });
}

```



3. 在项目Api中添加一个```Controller：IdentityController```,添加```[Authorize]```,开启认证

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

## Client

1. 安装包

```
Install-Package IdentityModel
```

2. 先从ids4服务拿到token

```c#
var client = new HttpClient();

var result = client.GetDiscoveryDocumentAsync("http://localhost:5600/").Result;

if (result.IsError)
{
    Console.WriteLine($"result:{result.Error}");
    //Console.ReadLine();
}

//请求token
var resultWithToken = client.RequestClientCredentialsTokenAsync(new ClientCredentialsTokenRequest
{
    Address = result.TokenEndpoint,
    ClientId = "ClientA",
    ClientSecret = "secret",
    Scope = "api1"
}).Result;

if (resultWithToken.IsError)
{
    Console.WriteLine($"resultWithToken:{resultWithToken.Error}");
}

Console.WriteLine($"token:{resultWithToken.Json}");
```


3. 带上token访问API

```c#

//调用认证api
var apiClient = new HttpClient();
apiClient.SetBearerToken(resultWithToken.AccessToken);

var response = apiClient.GetAsync("http://localhost:31773/identity").Result;
if (!response.IsSuccessStatusCode)
{
    Console.WriteLine($"response:{response.StatusCode}");
}
else
{
    var content = response.Content.ReadAsStringAsync().Result;
    Console.WriteLine($"content:{content}");
}

```

4. Fiddle拦截请求

可以看到request header中带上了```Authorization```

## [源码](https://github.com/thomerson/Demo/tree/main/DotnetCore/Demo.IDS4)

## 参考

* [https://github.com/IdentityServer/IdentityServer4](https://github.com/IdentityServer/IdentityServer4)
