## Quartz.NET

开源作业调度系统

### 概念

* 任务```Job```

* 触发器```Trigger```

* 调度器```Schedule```

* 调度工厂```StdSchedulerFactory```

* 线程池```SimpleThreadPool```

    默认个数为**10**个，代表一个Scheduler同一时刻并发的最多只能执行10个job，超过10个的job需要排队等待

### 触发器

* ```SimpleTrigger```

    在具体的时间点执行一次，或者在具体的时间点执行，并且以指定的间隔重复执行若干次。

    * 执行间隔
    
        * ```WithInterval(TimeSpan timeSpan)```：通用的间隔执行方法
        
        * ```WithIntervalInHours(int hours)```：以小时为间隔单位进行执行
        
        * ```WithIntervalInMinutes(int minutes)```：以分钟为间隔单位进行执行
        
        * ```WithIntervalInSeconds(int seconds)```：以秒为间隔单位进行执行

    * 执行时间

        * ```WithRepeatCount(int repeatCount)```：执行多少次以后结束

        * ```RepeatForever()```：永远执行

        * ```repeatMinutelyForever()```：一分钟执行一次(永远执行)

        * ```repeatMinutelyForever(int minutes)```：每隔几分钟执行一次(永远执行)

        * ```repeatMinutelyForTotalCount(int count, int minutes)``` 每隔几分钟执行一次(执行次数为count) 类似的还有秒、小时

* ```CalendarTrigger```
    
    日历相关
    
* ```DailyTimeTrigger```

    时间点的增、减、排除

	* ```OnEveryDay```：每天
	
	* ```OnMondayThroughFriday```:周一至周五,即工作日
	 
	* ```OnSaturdayAndSunday```:周六至周天，即休息日
	 
	* ```OnDaysOfTheWeek```:用数组的形式单独来指定一周中的哪几天
	 
	* ```StartingDailyAt```：表示开始于几点 （区别于前面的StartAt）
	 
	* ```EndingDailyAt```：表示结束于几点 （区别于前面的EndAt）


* ```CronTrigger```
    
    使用cron表达式代替硬编码

    [在线Cron表达式生成器](https://cron.qqe2.com/)


### Aop JobListener

在每次Job执行的前后，分别执行一段通用的业务


定义JobListener

```c#
public class MyAopListener : IJobListener
{
    public string Name
    {
        get
        {
            return "hello world";
        }
    }
    public void JobExecutionVetoed(IJobExecutionContext context)
    {

    }
    public void JobToBeExecuted(IJobExecutionContext context)
    {
        Console.WriteLine("执行前写入日志");
    }
    public void JobWasExecuted(IJobExecutionContext context, JobExecutionException jobException)
    {
        Console.WriteLine("执行后写入日志");
    }
}

```

添加JobListener

```c#
//1.创建Schedule
IScheduler scheduler = StdSchedulerFactory.GetDefaultScheduler();

//2.创建job (具体的job需要单独在一个文件中执行)
var job = JobBuilder.Create<HelloJob>().Build();

//3.配置trigger   1s执行一次
var trigger = TriggerBuilder.Create().WithSimpleSchedule(x => x.WithIntervalInSeconds(1)
                                                               .RepeatForever()).Build();
//AOP配置
scheduler.ListenerManager.AddJobListener(new MyAopListener(), GroupMatcher<JobKey>.AnyGroup());

//4.将job和trigger加入到作业调度池中
scheduler.ScheduleJob(job, trigger);

//5. 开始调度
scheduler.Start();
```

### 哑火MisFire策略

* ```WithMisfireHandlingInstructionFireAndProceed```

    **默认**

    错过的合并,于当前时间执行一次，不修改调度时间，按计划等待下一次调度

    **无论错过多少次，均在初次运行的时候，即当前时间执行一次，后续的执行仍按照原规律进行执行**

* ```WithMisfireHandlingInstructionIgnoreMisfires```

    错过的立即追赶，然后正常调度

    **错过多少次，初次执行的时候追赶多少次，追赶的次数的时间是按原规律执行的时间，然后按照原规律进行正常后续调度**

* ```WithMisfireHandlingInstructionDoNothing```

    错过的不管了，后续按照正常调度进行

    **错过的不管了，后续按照正常调度进行**


