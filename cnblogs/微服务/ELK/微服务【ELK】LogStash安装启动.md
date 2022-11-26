## windows安装启动

> 版本```logstash-8.4.1```

1. 修改```logstash```配置

在config文件夹中添加```logstash.conf```,参考```logstash-sample.conf```进行配置

* input

* filter

* output


```conf
# Sample Logstash configuration for creating a simple
# Beats -> Logstash -> Elasticsearch pipeline.

# Sample Logstash configuration for creating a simple
# Beats -> Logstash -> Elasticsearch pipeline.

input {
  rabbitmq {
	host => "localhost"
	password => "guest"
    port => 5672
	queue => ""
	ack => true
	arguments => { "x-ha-policy" => "all" }
	auto_delete => false
	automatic_recovery => true 
	connect_retry_interval => 1
	connection_timeout => 1000
	durable => true
	exchange => "rmq.target.orderapi"
	exchange_type => "topic"
	exclusive => false
	heartbeat => 60
	key => "orderapi-key"
	metadata_enabled => false
	passive => false
	prefetch_count => 256
	type => "orderapi"
	codec => "json"
	enable_metric => false
	id => input1
	add_field => {
		"key" => "value"
	}
  }
  
  rabbitmq {
	host => "localhost"
	password => "guest"
    port => 5672
	queue => ""
	ack => true
	arguments => { "x-ha-policy" => "all" }
	auto_delete => false
	automatic_recovery => true 
	connect_retry_interval => 1
	connection_timeout => 1000
	durable => true
	exchange => "rmq.target.productapi"
	exchange_type => "topic"
	exclusive => false
	heartbeat => 60
	key => "productapi-key"
	metadata_enabled => false
	passive => false
	prefetch_count => 256
	type => "productapi"
	codec => "json"
	enable_metric => false
	id => input2
	add_field => {
		"key" => "value"
	}
  }
}

output {
  if [type] == "orderapi"{
    elasticsearch {
      hosts => ["http://localhost:9200"]
      index => "orderapiapilog"
	  document_id => "%{@timestamp}"
      #user => "elastic"
      #password => "changeme"
	}
  }

  if [type] == "productapi"{
    elasticsearch {
      hosts => ["http://localhost:9200"]
      index => "productapiapilog"
	  document_id => "%{@timestamp}"
      #user => "elastic"
      #password => "changeme"
	}
  }
}


```

如果部署多个input和outout通过type和id来识别


2. 在bin路径下cmd执行命令

```logstash -f "C:\Software\logstash-8.4.1\config\logstash.conf"```

需要带上```logstash.conf```的配置参数

不然会提示```logstash.yml```的配置错误，事实上这个文件解压后默认都是注释掉的


