## 集成ASP.Net Core 

ASP.Net Core client 用户在访问网页时，没有授权的情况下自动跳转到IdentityServer的登录页面，输入用户名和密码后会获取token返回到ASP.Net Core的网页，实现了SSO单点登录的效果

登录步骤

1. MvcClient启动后Cookie中没有token会跳转到Ids4登录页面

2. Ids4验证用户名密码，写到Ids4验证通过重定向到MvcClient的```/signin-oidc```

3. ```/signin-oidc```会将token写入到MvcClient的cookie，然后跳转到MvcClient的启动页

这里其实MvcClient任意的地址没有token或者token失效（可以清理掉Ids4的cookie来测试）也会跳转到Ids4登录页面，登录成功后还会返回到原地址，不会返回到首页

![connect](https://gitee.com/thomerson/pictures/raw/master/2022/Ids4_connect.png)

这个图是监视请求

5001是Ids4的端口

7667是mvc client的端口

7667启动后会香5001发送一个连接请求```/connect/authorize```

退出登录的时候清理cookie会向5001发送一个结束session的请求```/connect/endsession```

从而实现两个系统之间通信

## IdentityServer

1. 安装包

```
Install-Package IdentityServer4
```


2. 添加UI

下载[Quickstart UI repo](https://github.com/IdentityServer/IdentityServer4.Quickstart.UI)

并将控制器、视图、模型和 CSS 放入 IdentityServer Web 应用程序。


3. 添加Config

添加```IdentityResources```,```ApiScopes```,```Clients```

```c#
public static IEnumerable<IdentityResource> IdentityResources =>
     new List<IdentityResource>
    {
        new IdentityResources.OpenId(),
        new IdentityResources.Profile(),
    };

public static IEnumerable<ApiScope> ApiScopes =>
  new List<ApiScope>
  {
        new ApiScope("api1", "My API")
  };

public static IEnumerable<Client> Clients =>
    new List<Client>
    {
        // machine to machine client
        //new Client
        //{
        //    ClientId = "client",
        //    ClientSecrets = { new Secret("secret".Sha256()) },

        //    AllowedGrantTypes = GrantTypes.ClientCredentials,
        //    // scopes that client has access to
        //    AllowedScopes = { "api1" }
        //},
        
        // interactive ASP.NET Core MVC client
        new Client
        {
            ClientId = "mvc",
            ClientSecrets = { new Secret("secret".Sha256()) },

            AllowedGrantTypes = GrantTypes.Code,
            
            // where to redirect to after login
            RedirectUris = { "https://localhost:7667/signin-oidc" },

            // where to redirect to after logout
            PostLogoutRedirectUris = { "https://localhost:7667/signout-callback-oidc" },

            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile
            }
        }
    };

```

4. 添加IdentityServer中间件

```c#
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    //services.AddRazorPages();
    services.AddControllersWithViews();

    var builder = services.AddIdentityServer()
       .AddInMemoryIdentityResources(IdentityServerConfig.IdentityResources)
       .AddInMemoryApiScopes(IdentityServerConfig.ApiScopes)
       .AddInMemoryClients(IdentityServerConfig.Clients)
       .AddTestUsers(TestUsers.Users);

    builder.AddDeveloperSigningCredential();
}

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
    }


    app.UseStaticFiles();

    app.UseRouting();

    app.UseIdentityServer();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        //endpoints.MapRazorPages();
        endpoints.MapDefaultControllerRoute();
    });
}

```


此时启动网站可以看到IdentityServer的Home页面

## MvcClient

1. 安装包

```
Install-Package Microsoft.AspNetCore.Authentication.OpenIdConnect
```

2. 添加Control view

```c#

public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }

    public IActionResult Logout()
    {
        return SignOut("Cookies", "oidc");
    }
}

```

3. Service注册```Authentication```

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();

    JwtSecurityTokenHandler.DefaultMapInboundClaims = false;

    services.AddAuthentication(options =>
    {
        options.DefaultScheme = "Cookies";
        options.DefaultChallengeScheme = "oidc";
    })
    .AddCookie("Cookies")
    .AddOpenIdConnect("oidc", options =>
    {
        options.Authority = "http://localhost:5001";

        options.ClientId = "mvc";
        options.ClientSecret = "secret";
        options.ResponseType = "code";

        options.SaveTokens = true;
        options.RequireHttpsMetadata = false;
    });

    services.Configure<CookiePolicyOptions>(options =>
    {
        options.MinimumSameSitePolicy = SameSiteMode.Unspecified;
        options.Secure = CookieSecurePolicy.SameAsRequest;
        options.OnAppendCookie = cookieContext =>
            AuthenticationHelpers.CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            AuthenticationHelpers.CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
    }

    app.UseCookiePolicy();

    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();

    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        //endpoints.MapRazorPages();
        endpoints.MapDefaultControllerRoute()
        .RequireAuthorization();
    });
}

```

先启动Ids4，然后再启动MvcClient，提示登录，可能会遇到一些错误，例如demo中没使用https登录跳转错误，下面是解决的方法



## 遇到的问题

1. 登录后跳转到signin-oidc提示```Correlation failed.: Unknown location```,如图

![错误信息](https://gitee.com/thomerson/pictures/raw/master/2022/Ids4_signin-oidc.PNG)

这是由谷歌内核浏览器 cookie 策略引起的，参考 [Cookie 的 SameSite 属性](http://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html)，用来防止 CSRF 攻击和用户追踪

所以解决方法是

*. 使用Https

~现实是很多都是内网使用，不会用https的

*. 设置浏览器

~现实是用户才不会搭理你

*. 在MvcClient配置```SameSite = SameSiteMode.Unspecified;```

```c#
// 在ConfigureServices添加

services.Configure<CookiePolicyOptions>(options =>
{
    options.MinimumSameSitePolicy = SameSiteMode.Unspecified;
    options.Secure = CookieSecurePolicy.SameAsRequest;
    options.OnAppendCookie = cookieContext =>
        AuthenticationHelpers.CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    options.OnDeleteCookie = cookieContext =>
        AuthenticationHelpers.CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
});



// 在Configure中添加

app.UseCookiePolicy();

// AuthenticationHelpers

public static class AuthenticationHelpers
{
    public static void CheckSameSite(HttpContext httpContext, CookieOptions options)
    {
        if (options.SameSite == SameSiteMode.None)
        {
            var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
            if (!httpContext.Request.IsHttps || DisallowsSameSiteNone(userAgent))
            {
                // For .NET Core < 3.1 set SameSite = (SameSiteMode)(-1)
                options.SameSite = SameSiteMode.Unspecified;
            }
        }
    }

    public static bool DisallowsSameSiteNone(string userAgent)
    {
        // Cover all iOS based browsers here. This includes:
        // - Safari on iOS 12 for iPhone, iPod Touch, iPad
        // - WkWebview on iOS 12 for iPhone, iPod Touch, iPad
        // - Chrome on iOS 12 for iPhone, iPod Touch, iPad
        // All of which are broken by SameSite=None, because they use the iOS networking stack
        if (userAgent.Contains("CPU iPhone OS 12") || userAgent.Contains("iPad; CPU OS 12"))
        {
            return true;
        }

        // Cover Mac OS X based browsers that use the Mac OS networking stack. This includes:
        // - Safari on Mac OS X.
        // This does not include:
        // - Chrome on Mac OS X
        // Because they do not use the Mac OS networking stack.
        if (userAgent.Contains("Macintosh; Intel Mac OS X 10_14") &&
            userAgent.Contains("Version/") && userAgent.Contains("Safari"))
        {
            return true;
        }

        // Cover Chrome 50-69, because some versions are broken by SameSite=None, 
        // and none in this range require it.
        // Note: this covers some pre-Chromium Edge versions, 
        // but pre-Chromium Edge does not require SameSite=None.
        if (userAgent.Contains("Chrome/5") || userAgent.Contains("Chrome/6"))
        {
            return true;
        }

        return false;
    }
}

```

## 源码

[Demo.IDS4.ASPNet](https://github.com/thomerson/Demo/tree/main/DotnetCore/Demo.IDS4.ASPNet)

## 参考

* [IdentityServer4.Quickstart](https://github.com/IdentityServer/IdentityServer4.Quickstart.UI)

* [How To Prepare Your IdentityServer For Chrome’s SameSite Cookie Changes – And How To Deal With Safari, Nevertheless](https://www.thinktecture.com/en/identityserver/prepare-your-identityserver/)




