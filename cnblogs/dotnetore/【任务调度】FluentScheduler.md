## FluentScheduler

轻量级的定时任务工具，时间设置很方便，很适合简单的定时任务开发

比起Hangfire不足在于没有做数据持久化和可视化

最新的版本是standard的，Framework和Core都可以直接install后使用

nuget安装```FluentScheduler```

```
install-package FluentScheduler
```

[文档](https://fluentscheduler.github.io/)

[Github](https://github.com/fluentscheduler/FluentScheduler)

## 重要的概念

* Job：单个执行的任务，集成```IJob```接口，每个job实现```Execute```方法

```c#
    public class MyJob : IJob
    {
        public void Execute()
        {
            //Thread.Sleep(5000);
            Logger.Write($"{nameof(MyJob)}:{System.DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss")}");
        }
    }
```

* Registry：在Registry中定义Job的定时规则，例如每天9点执行哪个Job，可以定义多个
```c#

    public class MyRegistry : Registry
    {
        public MyRegistry()
        {
            // 默认情况下，该库允许计划与先前触发的同一计划的执行并行运行,禁止同一任务并发执行，
            // 等待上一个任务执行完再执行下一个任务
            // 或者用NonReentrant单个job设置
            NonReentrantAsDefault();

            // Schedule an IJob to run at an interval
            //Schedule<MyJob>()
            //    //.NonReentrant()
            //    .ToRunNow().AndEvery(2).Seconds();

            // Schedule an IJob to run once, delayed by a specific time interval
            // 只执行1次
            //Schedule<MyJob>().ToRunOnceIn(5).Seconds();

            //// Schedule a simple job to run at a specific time
            //// 每天21:15
            //Schedule(() => Logger.Write("It's 9:15 PM now.")).ToRunEvery(1).Days().At(21, 15);

            //// Schedule a more complex action to run immediately and on an monthly interval
            //// 每月一次
            //Schedule<MyComplexJob>().ToRunNow().AndEvery(1).Months().OnTheFirst(DayOfWeek.Monday).At(3, 0);

            //// Schedule a job using a factory method and pass parameters to the constructor.
            //// 10秒每次
            //Schedule(() => new MyComplexJob("Foo", DateTime.Now)).ToRunNow().AndEvery(10).Seconds();

            //// Schedule multiple jobs to be run in a single schedule
            //// 现在开始5分钟每次
            Schedule<MyJob>().AndThen<MyOtherJob>().ToRunNow().AndEvery(5).Minutes();

            // 现在开始  然后每周一次
            Schedule<MyJob>().ToRunEvery(0).Weeks().On(DayOfWeek.Monday).At(14, 0);

            // Every "one" weeks
            //Schedule<MyJob>().ToRunEvery(1).Weeks().On(DayOfWeek.Monday).At(14, 0);
        }
    }
```
* JobManager: Job管理，初始化Registry，Job的启动停止事件注册，异常处理，停止服务

```c#

// 单个注册
JobManager.Initialize(new MyRegistry());

// 多个注册
//扫描当前程序集中实现了Registry的类
var registrys = Assembly.GetExecutingAssembly().GetTypes()
                       .Where(t => !t.IsInterface && !t.IsSealed && !t.IsAbstract && typeof(Registry).IsAssignableFrom(t))
                       .Select(s => s.Assembly.CreateInstance(s.FullName) as Registry)?.ToArray();

// 注册同步服务
JobManager.Initialize(registrys);

// 启动停止异常事件
JobManager.JobStart += info => Logger.Information($"{info.Name}: started");
JobManager.JobEnd += info => Logger.Information($"{info.Name}: ended ({info.Duration})");
JobManager.JobException += info => Logger.Error("An error just happened with a scheduled job: " + info.Exception);

// 停止JobManager
JobManager.Stop();
JobManager.StopAndBlock(); //等待正在运行的作业完成后停止


```


## 源码地址

[Demo.FluentScheduler](https://github.com/thomerson/Demo/tree/main/DotnetCore/Demo.FluentScheduler)

