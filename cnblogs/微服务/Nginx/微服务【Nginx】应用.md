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

## CORS跨域

1. 使用反向代理解决跨域

2. 配置 header 解决跨域


```conf
server {
  listen       80;
  server_name  be.sherlocked93.club;
  
	add_header 'Access-Control-Allow-Origin' $http_origin;   # 全局变量获得当前请求origin，带cookie的请求不支持*
	add_header 'Access-Control-Allow-Credentials' 'true';    # 为 true 可带上 cookie
	add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';  # 允许请求方法
	add_header 'Access-Control-Allow-Headers' $http_access_control_request_headers;  # 允许请求的 header，可以为 *
	add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
	
  if ($request_method = 'OPTIONS') {
		add_header 'Access-Control-Max-Age' 1728000;   # OPTIONS 请求的有效期，在有效期内不用发出另一条预检请求
		add_header 'Content-Type' 'text/plain; charset=utf-8';
		add_header 'Content-Length' 0;
    
		return 204;                  # 200 也可以
	}
  
	location / {
		root  /usr/share/nginx/html/be;
		index index.html;
	}
}


```

## 开启 gzip 压缩

gzip 是一种常用的网页压缩技术，传输的网页经过 gzip 压缩之后大小通常可以变为原来的一半甚至更小（官网原话），更小的网页体积也就意味着带宽的节约和传输速度的提升，特别是对于访问量巨大大型网站来说，每一个静态资源体积的减小，都会带来可观的流量与带宽的节省。


⭐[GZip网站检测工具](https://tool.chinaz.com/gzips/)

0. 浏览器需要支持gzip压缩

    IE5 之后所有的浏览器都支持了，是现代浏览器的默认设置

    请求头中包含```Accept-Encoding: gzip```


    response返回```content-encoding: gzip```


1. 添加```gzip.conf```配置

```conf
# /etc/nginx/conf.d/gzip.conf

gzip on; # 默认off，是否开启gzip
gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

# 上面两个开启基本就能跑起了，下面的愿意折腾就了解一下
gzip_static on;
gzip_proxied any;
gzip_vary on;
gzip_comp_level 6;
gzip_buffers 16 8k;
# gzip_min_length 1k;
gzip_http_version 1.1;


# gzip_types：要采用 gzip 压缩的 MIME 文件类型，其中 text/html 被系统强制启用；
# gzip_static：默认 off，该模块启用后，Nginx 首先检查是否存在请求静态文件的 gz 结尾的文件，如果有则直接返回该 .gz 文件内容；
# gzip_proxied：默认 off，nginx做为反向代理时启用，用于设置启用或禁用从代理服务器上收到相应内容 gzip 压缩；
# gzip_vary：用于在响应消息头中添加 Vary：Accept-Encoding，使代理服务器根据请求头中的 Accept-Encoding 识别是否启用 gzip 压缩；
# gzip_comp_level：gzip 压缩比，压缩级别是 1-9，1 压缩级别最低，9 最高，级别越高压缩率越大，压缩时间越长，建议 4-6；
# gzip_buffers：获取多少内存用于缓存压缩结果，16 8k 表示以 8k*16 为单位获得；
# gzip_min_length：允许压缩的页面最小字节数，页面字节数从header头中的 Content-Length 中进行获取。默认值是 0，不管页面多大都压缩。建议设置成大于 1k 的字节数，小于 1k 可能会越压越大；
# gzip_http_version：默认 1.1，启用 gzip 所需的 HTTP 最低版本；


```

这个配置可以插入到 http 模块整个服务器的配置里，也可以插入到需要使用的虚拟主机的 server 或者下面的 location 模块中

```gzip_min_length 1k```建议加上，不然文件太小可能压缩后体积反而会增加


## 适配 PC 或移动设备

如果PC网页和H5网页分开的UI的话，可以用if判断分别对应不同的路径

```conf
# /etc/nginx/conf.d/fe.sherlocked93.club.conf

server {
  listen 80;
	server_name fe.sherlocked93.club;

	location / {
		root  /usr/share/nginx/html/pc;
    if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
      root /usr/share/nginx/html/mobile;
    }
		index index.html;
	}
}


```


## 请求过滤

```conf
# 非指定请求全返回 403
if ( $request_method !~ ^(GET|POST|HEAD)$ ) {
  return 403;
}

location / {
  # IP访问限制（只允许IP是 192.168.0.2 机器访问）
  allow 192.168.0.2;
  deny all;
  
  root   html;
  index  index.html index.htm;
}


```
<!-- 
TODO
https://www.nginx.org.cn/article/detail/545/ -->