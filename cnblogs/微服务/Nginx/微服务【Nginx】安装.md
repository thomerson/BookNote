## windows安装

1. 下载

[官网下载地址](http://nginx.org/en/download.html)

⭐ 安装目录不能有中文

⭐ 80端口不能被占用

2. 在nginx路径下cmd启动

打开```localhost:port```可以看到nginx的启动页面


## docker安装


### 80端口被占用

1. 错误信息

```
bind() to 0.0.0.0:80 failed (10013: An attempt was made to access a socket in a way forbidden by its access permissions)
```


2. 查看端口占用命令

```shell
# 端口使用情况查询
netstat -aon | findstr :80

# 端口task
tasklist|findstr “12824”

```

关掉重新打开

3. 修改占用的端口

打开```\conf\nginx.conf```编辑


