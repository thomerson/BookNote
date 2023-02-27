## Hangfire

组成

* 客户端

* 作业存储

    * sql server
    * redis

* 服务端

![Hangfire](https://static.bookstack.cn/projects/Hangfire-zh-official/153a9d5156afd2e9.png)


后台作业

* ```fire-and-forget```
    
    自助调用

* ```delayed```

    在一段时间后执行调用

*  ```recurring```

    按小时，每天执行方法等


Job类型

* ```Recurring Job```

    周期性定时任务

    使用Cron表达式来设定一个执行周期时间，每到设定时间就被激活执行一次

* ```Fire and Forget```

    执行单次任务

* ```Continouus Job```

    连续顺序执行任务


* ```Schedule Job```

    定时单次任务

## .net6集成

1. 安装nuget包

```powershell
install-package hangfire
```


2. 注册hangfire

```c#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddHangfire(x => x.UseSqlServerStorage("<connection string>"));
builder.Services.AddHangfireServer();  // add hangfire



var app = builder.Build();

// Configure the HTTP request pipeline.

app.UseAuthorization();

app.UseHangfireDashboard(); // use hangfire

app.MapControllers();

app.Run();
```

默认情况下，只有本地访问权限才能使用Hangfire仪表盘。为了远程访问必须配置 仪表盘授权 

如果本地没有表的话会生成表，空项目需要先创建数据库

访问地址```/hangfire```查看调度控制面板


### 一些常见任务写法

```c#

//支持基于队列的任务处理：任务执行不是同步的，而是放到一个持久化队列中，以便马上把请求控制权返回给调用者。
var jobId = BackgroundJob.Enqueue(()=>Console.WriteLine("队列任务执行了！"));
 
//延迟任务执行：不是马上调用方法，而是设定一个未来时间点再来执行，延迟作业仅执行一次
var jobId = BackgroundJob.Schedule（()=>Console.WriteLine("一天后的延迟任务执行了！"),TimeSpan .FromDays(1));//一天后执行该任务
 
//循环任务执行：一行代码添加重复执行的任务，其内置了常见的时间循环模式，也可基于CRON表达式来设定复杂的模式。【用的比较的多】
RecurringJob.AddOrUpdate(()=>Console.WriteLine("每分钟执行任务！"), Cron.Minutely); //注意最小单位是分钟
 
//延续性任务执行：类似于.NET中的Task,可以在第一个任务执行完之后紧接着再次执行另外的任务
BackgroundJob.ContinueWith(jobId,()=>Console.WriteLine("连续任务！"));
```

<!-- TODO -->
<!-- https://www.cnblogs.com/for-easy-fast/p/14512594.html -->

## 添加HttpJob

<!-- https://github.com/yuzd/Hangfire.HttpJob -->

<!-- https://www.bookstack.cn/read/Hangfire-zh-official/README.md -->