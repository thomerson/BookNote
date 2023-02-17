## 隔离

```Bulkhead Isolation```

当系统的一处出现故障时，可能促发多个失败的调用，很容易耗尽主机的资源(如 CPU)。

下游系统出现故障可能导致上游的故障的调用，甚至可能蔓延到导致系统崩溃。所以要将可控的操作限制在一个固定大小的资源池中，以隔离有潜在可能相互影响的操作。

策略是最多允许 12 个线程并发执行，如果执行被拒绝，则执行回调。

```c#

Console.WriteLine($"隔离策略");
Policy.Bulkhead(12, context => // 最多允许 12 个线程并发执行，如果执行被拒绝，则执行回调
{
    // do something
}).Execute(ExecuteMockRequest);
```