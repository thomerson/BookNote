
## 策略包

```Policy Wrap```

一种操作会有多种不同的故障，而不同的故障处理需要不同的策略。这些不同的策略必须包在一起，作为一个策略包，才能应用在同一种操作上。

```c#
var policyWrap = Policy
  .Wrap(fallback, cache, retry, breaker, timeout, bulkhead);
// (wraps the policies around any executed delegate: fallback outermost ... bulkhead innermost)
policyWrap.Execute(...)

// Define a standard resilience strategy ...
PolicyWrap commonResilience = Policy.Wrap(retry, breaker, timeout);

// ... then wrap in extra policies specific to a call site, at that call site:
Avatar avatar = Policy<UserAvatar>
   .Handle<FooException>()
   .Fallback<Avatar>(Avatar.Blank)
   .Wrap(commonResilience)
   .Execute(() => { /* get avatar */ });

// Share commonResilience, but wrap different policies in at another call site:
Reputation reps = Policy
   .Handle<FooException>()
   .Fallback<Reputation>(Reputation.NotAvailable)
   .Wrap(commonResilience)
   .Execute(() => { /* get reputation */ });  

```