## Kibana安装启动

1. 配置```kibana.yml```

```yml
elasticsearch.hosts: ["http://localhost:9200"]
i18n.locale: "zh-CN"
```

2. bin路径下cmd执行```kibana.bat```

es和kibana对内存和磁盘剩余空间要求比较高，建议磁盘至少空
%，尽量不开占内存的大软件


3. 浏览器打开地址```http://localhost:5601/```

没有加载完成会有提醒



