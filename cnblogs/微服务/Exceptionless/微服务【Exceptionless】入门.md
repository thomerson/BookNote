## ExceptionLess

开源日志框架

[官网](https://exceptionless.com/)
[说明文档](https://github.com/exceptionless/Exceptionless/wiki/)
[Github](https://github.com/exceptionless/Exceptionless/)



## 安装使用

```powershell
Install-Package Install-Package Exceptionless.AspNetCore
```

注册

```c#
using Exceptionless;
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
 
        app.UseExceptionless("[你的API密钥]");
        app.UseMvc();
 }
```

使用

```c#

// GET api/values/{id}
[HttpGet("{id}")]
public ActionResult<string> Get(int id)
{
    try
    {
        throw new Exception();
    }
    catch (Exception ex)
    {
        ex.ToExceptionless().Submit();
    }
    return $"value {id}";
}

```

## 本地部署


依赖

1. Java JDK


## 后台运行服务







https://github.com/exceptionless/Exceptionless/wiki/Self-Hosting



## Exceptionless 日志查询

https://github.com/exceptionless/Exceptionless/wiki/Filtering-Searching

