## 集群

1. 配置服务器组，在http{}节点之间添加upstream配置。

⭐不要写localhost，不然访问速度会很慢

```yaml
upstream nginxCluster{
	server 127.0.0.1:8080;  #服务器8080
	server 127.0.0.1:8081;  #服务器8081
}
```

2. proxy_pass配置反向代理地址

此处```http://```不能少，后面的地址要和第一步upstream定义的名称保持一致

```yaml
location /{  
	    proxy_pass http://nginxCluster;  
} 
```


## 负载均衡

将服务器接收到的请求按照规则分发的过程


1. 轮询

	**默认**

	每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除

2. weight权重

weight和访问比率成正比，用于后端服务器性能不均的情况默认选项，当weight不指定时，各服务器weight相同， (weight=1)

```yaml
upstream nginxCluster{
	server 127.0.0.1:8080 weight=1;  #服务器8080
	server 127.0.0.1:8081 weight=2;  #服务器8081
	server 127.0.0.1:8082 weight=3;  #服务器8081
}
```

数字越大，表明请求到的机会越大

3. ip_hash

每个请求按访问ip的hash值分配，这样同一客户端连续的Web请求都会被分发到同一服务器进行处理，可以解决session的问题。当后台服务器宕机时，会自动跳转到其它服务器。

```yaml
upstream nginxCluster{
	ip_hash;
	server 127.0.0.1:8080;  #服务器8080
	server 127.0.0.1:8081;  #服务器8081
	server 127.0.0.1:8082;  #服务器8081
}
```
