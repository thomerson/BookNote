## 持久化

支持多种数据库

以sql server为例


1. 执行脚本，创建table

[脚本](https://github.com/quartznet/quartznet/blob/main/database/tables/tables_sqlServer.sql)

表解释

* qrtz_blob_triggers : 以Blob 类型存储的触发器。 

* qrtz_calendars：存放日历信息， quartz可配置一个日历来指定一个时间范围。 

* qrtz_cron_triggers：存放cron类型的触发器。 

* qrtz_fired_triggers：存放已触发的触发器。 

* qrtz_job_details：存放一个jobDetail信息。 

* qrtz_job_listeners：job**监听器**。 

* qrtz_locks： 存储程序的悲观锁的信息(假如使用了悲观锁)。 

* qrtz_paused_trigger_graps：存放暂停掉的触发器。 

* qrtz_scheduler_state：调度器状态。 

* qrtz_simple_triggers：存放简单触发器的信息。 

* qrtz_trigger_listeners：触发器监听器。 

* qrtz_triggers：将Trigger和job进行关联的表。

2. 代码配置数据库

```c#

var properties = new NameValueCollection();
//SQLServer版本
properties.Add("quartz.dataSource.myDS.provider", "SqlServer-20");
//表名前缀(可有可无)
//properties.Add("quartz.jobStore.tablePrefix", "QRTZ_");
//数据库连接字符串
properties.Add("quartz.dataSource.myDS.connectionString", "Data Source=.;Initial Catalog=quartz;User ID=sa;Password=123456");
//properties.Add("quartz.dataSource.myDS.connectionString", "Server =.;Database = quartz;Trusted_Connection =True;");
//JobStore设置（JobStoreTX: 带有事务；JobStoreCMT：不带有事务）
//存储类型
properties.Add("quartz.jobStore.type", "Quartz.Impl.AdoJobStore.JobStoreTX, Quartz");
//数据源名称
properties.Add("quartz.jobStore.dataSource", "myDS");
//驱动类型
properties.Add("quartz.jobStore.driverDelegateType", "Quartz.Impl.AdoJobStore.StdAdoDelegate, Quartz");

//cluster 集群指定
//properties["quartz.jobStore.clustered"] = "true";
//properties["quartz.scheduler.instanceId"] = "AUTO";
```

3. 向数据库中持久化作业，并开启调度

```c#
// properties
var factory = new StdSchedulerFactory(properties);
IScheduler scheduler = factory.GetScheduler();
var job = JobBuilder.Create<HelloJob4>()
                    .WithIdentity("ypfJob1", "ypfJobGroup1")
                    .Build();
var trigger = TriggerBuilder.Create()
                     .WithIdentity("ypfTrigger1", "ypfTriggerGroup1")
                    .WithCronSchedule("/3 * * * * ?")
                    .Build();
if (!scheduler.CheckExists(job.Key))
{
    scheduler.ScheduleJob(job, trigger);
}
scheduler.Start();
```