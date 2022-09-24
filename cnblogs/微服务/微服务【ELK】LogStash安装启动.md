## windows安装启动

> 版本```logstash-8.4.1```

1. 修改```logstash```配置

在config文件夹中添加```logstash.conf```,参考```logstash-sample.conf```进行配置


2. 在bin路径下cmd执行命令

```logstash -f "C:\Software\logstash-8.4.1\config\logstash.conf"```

需要带上```logstash.conf```的配置参数

不然会提示```logstash.yml```的配置错误，事实上这个文件解压后默认都是注释掉的


