## Kibana无法启动

报错信息

![Kibana_启动错误](https://img2022.cnblogs.com/blog/999484/202209/999484-20220925144803051-1068940635.png)

``` [.kibana] Action failed with '[index_not_green_timeout] Timeout waiting for the status of the [.kibana_8.4.1_001] index to become 'green' Refer to https://www.elastic.co/guide/en/kibana/8.4/resolve-migrations-failures.html#_repeated_time_out_requests_that_eventually_fail for information on how to resolve the issue.'. Retrying attempt 5 in 32 seconds.```

原因：磁盘空间使用率超过85%

