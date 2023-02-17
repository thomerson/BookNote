## 断路

```Circuit-breaker```

快速失败

当系统遇到严重问题时，快速回馈失败比让调用者等待要好，限制系统出错的体量，有助于系统恢复。

```c#

Console.WriteLine($"断路策略");


Policy.Handle<HttpRequestException>()
    .CircuitBreaker(2, TimeSpan.FromMinutes(1)) //策略是，当系统出现两次某个异常时，就停下来，等待 1 分钟后再继续
    .Execute(ExecuteMockRequest);



```

