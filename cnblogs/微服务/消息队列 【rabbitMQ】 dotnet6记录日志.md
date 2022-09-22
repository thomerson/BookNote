[RabbitMQ安装]()

## dotnet6向RabbitMQ发送日志

1. 安装```NLOG```包

```
install-package NLog
install-package NLog.Web.AspNetCore
install-package NLog.RabbitMQ.Target

```

2. 配置```nlog.config```

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  autoReload="true"
	  internalLogLevel="Warn"
	  internalLogFile="logs/internal-nlog.txt">
	<!--发送到RabbitMQ-->
	<extensions>
		<add assembly="Nlog.RabbitMQ.Target" />
	</extensions>
	<variable name="rmqHost" value="localhost" />
	<variable name="rmqUser" value="guest" />
	<variable name="rmqPassword" value="guest" />
	<variable name="rmqvHost" value="/" />
	<targets async="true">
		<target name="logstash"
		xsi:type="RabbitMQ"
		username="${rmqUser}"
		password="${rmqPassword}"
		hostname="${rmqHost}"
		exchange="rmq.target.orderapi"
		exchangeType="topic"
		topic="orderapi-log"
		port="5672"
		vhost="${rmqvHost}"
		useJSON ="true"
		layout="${longdate}|${logger}|${uppercase:${level}}|${message} ${exception}"
		UseLayoutAsMessage="true"
      >
			<field key="fieldFromConfig" name="Field From Config" layout="${machinename}"/>
			<field key="EmployeeName" name="Employee Name" layout="Overriden From Config"/>
			<field key="EmployeeID" name="" layout="Overriden From Config"/>
		</target>
	</targets>


	<rules>
		<logger name="*" minlevel="Trace" writeTo="logstash" />
	</rules>
</nlog>

```

3. ```program```添加Nlog

```c#

using NLog;
using NLog.Web;

var builder = WebApplication.CreateBuilder(args);

// ...

builder.Logging.AddNLog("nlog.config");
builder.Host.UseNLog();

var app = builder.Build();

```

4. 查看RabbitMQ配置页面

可以看到配置中添加的```topic```

记录日志后overview中message图有变动，记录成功

