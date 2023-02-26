## dotnet6集成

<!-- https://www.codenong.com/cs109311634/ -->

1. 安装nuget包

```powershell
Install-Package Quartz

Install-Package Quartz.Serialization.Json # jsonSerialization
```

2. 配置

    有两种方式
    
    * ```NameValueCollection``` 

    ```c#
    var properties = new NameValueCollection();

	IScheduler scheduler = await SchedulerBuilder.Create(properties)
		// default max concurrency is 10
		.UseDefaultThreadPool(x => x.MaxConcurrency = 5)
		// this is the default 
		// .WithMisfireThreshold(TimeSpan.FromSeconds(60))
		.UsePersistentStore(x =>
		{
			// force job data map values to be considered as strings
			// prevents nasty surprises if object is accidentally serialized and then 
			// serialization format breaks, defaults to false
			x.UseProperties = true;
			x.UseClustering();
			// there are other SQL providers supported too 
			x.UseSqlServer("my connection string");
			// this requires Quartz.Serialization.Json NuGet package
			x.UseJsonSerializer();
		})
		// job initialization plugin handles our xml reading, without it defaults are used
		// requires Quartz.Plugins NuGet package
		.UseXmlSchedulingConfiguration(x =>
		{
			x.Files = new[] { "~/quartz_jobs.xml" };
			// this is the default
			x.FailOnFileNotFound = true;
			// this is not the default
			x.FailOnSchedulingError = true;
		})
		.BuildScheduler();
	
	await scheduler.Start();
    ```


    * 配置文件

3. 注册服务

```c#
//Quartz注册服务
builder.Services.AddSingleton<ISchedulerFactory, StdSchedulerFactory>();

```


