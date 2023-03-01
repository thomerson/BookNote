## 静态资源

静态配置文件处理
由于Nginx性能很高，对于常用的静态资源，可直接交由Nginx进行访问处理

```yaml
server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        location /image/ {
            root   /usr/local/myImage/;
            autoindex on;
        }
    }
```
这个配置表示输入 ```localhost:80/image/``` 时会访问本机的```/usr/local/myImage/image/``` 目录

重启后就可以访问本机目录的静态图片了



## 反向代理

修改nginx.conf文件，查看server 节点，相当于一个代理服务器，可以配置多个

localhost时转到tomcat时。修改两个地方：

```yaml
 server_name  exam_qf;
 location / {
	    proxy_pass http://127.0.0.1:8080;  # 表示代理路径，相当于转发
  }
```

## 动静分离

```yaml
location ~ \.(html|js|css|png|gif)$ {  
	root D:/static;   #可以是nginx 外部的文件
}
location /{  
	proxy_pass http://127.0.0.1:8080;  
} 


# ~ 表示正则匹配，后面的内容可以是正则表达式匹配
# . 点表示任意字符
# *表示一个或多个字符
# \. 是转移字符
# |表示或
# $表示结尾

# 整个配置代表括号晨面的后缀请求都由nginx处理。

```

nginx对location访问优先是以精确优先为原则，故将精确细的请求放在前面。这样可以完成基本的动静分离配置。

