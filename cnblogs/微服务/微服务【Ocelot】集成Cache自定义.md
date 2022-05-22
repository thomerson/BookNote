## ```IOcelotCache```

通过继承接口```IOcelotCache```，然后注册到容器即可


1. 实现```IOcelotCache```
```c#

using Ocelot.Cache;

namespace DemoOcelot.midware
{
    public class OcelotCache<CachedResponse> : IOcelotCache<CachedResponse>
    {
        private class CacheModel
        {
            public string Region { get; set; }
            public TimeSpan TtlSeconds { get; set; }
            public CachedResponse CachedResponse { get; set; }
        }

        private static Dictionary<string, CacheModel> _cache = new Dictionary<string, CacheModel>();

        public void Add(string key, CachedResponse value, TimeSpan ttl, string region)
        {
            _cache[key] = new CacheModel() { Region = region, TtlSeconds = ttl, CachedResponse = value };
        }

        public void AddAndDelete(string key, CachedResponse value, TimeSpan ttl, string region)
        {
            _cache[key] = new CacheModel() { Region = region, TtlSeconds = ttl, CachedResponse = value };
        }

        public void ClearRegion(string region)
        {
            throw new NotImplementedException();
        }

        public CachedResponse Get(string key, string region)
        {
            if (_cache.ContainsKey(key))
            {
                return _cache[key].CachedResponse;
            }
            return default;
        }
    }
}


```

2. 将缓存类注册到容器中

```c#

builder.ConfigureServices(s =>
{
    s.AddOcelot()
    .AddPolly();

    s.AddSingleton<IOcelotCache<CachedResponse>, OcelotCache<CachedResponse>>(); 
   
})

```

