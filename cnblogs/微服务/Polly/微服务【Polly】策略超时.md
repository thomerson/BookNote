## 超时


```Timeout```

当系统超过一定时间的等待,就可以判断不可能会有成功的结果，直接返回，避免系统长时间做无谓的等待。


```c#

Console.WriteLine($"超时策略");
Policy.Timeout(30, onTimeout: (context, timespan, task) => //30s超时
{
    // do something
}).Execute(ExecuteMockRequest);



```
