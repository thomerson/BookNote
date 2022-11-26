## windows安装启动

1. 安装JAVA环境

```ElasticSearch```是基于```lucence```开发的，也就是运行需要java jdk支持。

记得配置Java环境变量

注意⭐

ElasticSearch启动需要很大的**磁盘空间**，我是装在系统盘的，只剩10G总是启动失败


2. 下载```elasticsearch```最新版本解压后直接在bin路径下cmd执行```elasticsearch.bat```

错误提醒```exception during geoip databases update```

* 原因
此版本将GeoIp功能默认开启了采集。在默认的启动下是会去官网的默认地址下获取最新的Ip的GEO信息

* 解决方案
在```elasticsearch.yml```的末尾添加配置```ingest.geoip.downloader.enabled: false```


3. 浏览器打开地址```http://localhost:9200/```验证启动成功


打开后需要输入用户名密码，如果不需要设置可以在```elasticsearch.yml```将```xpack.security.enabled: true```设置```为false```

<!-- TODO -->
修改密码一直不成功

错误信息

```ERROR: Failed to determine the health of the cluster. Cluster health is currently RED.```


## ElasticSearch安装为Windows服务

<!-- TODO -->

[Windows服务](https://blog.csdn.net/lilian129/article/details/124477099)
