## Prometheus安装

* windows

* docker

### windows安装


1.下载

[官网下载地址](https://prometheus.io/download/)


需要下载

* prometheus

* alertmanager

* blackbox_exporter

* consul_exporter

* graphite_exporter

* haproxy_exporter

* memcached_exporter

* mysqld_exporter

* node_exporter

* promlens

* pushgateway

* statsd_exporter



#### server安装

1. 下载

2. 配置```prometheus.yaml```


```yaml
global:
  scrape_interval: 5s # 抓取频率

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
```
3. 启动

    打开地址```http://localhost:9090/```

    



### docker安装

<!-- TODO -->



<!-- https://www.cnblogs.com/chanshuyi/p/01_head_first_of_prometheus.html -->

<!-- https://www.cnblogs.com/chenqionghe/p/10494868.html -->


## promql
<!-- https://p8s.io/docs/promql/data-model/ -->