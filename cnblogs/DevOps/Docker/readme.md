* 入门
* 常用命令
* 启动微服务
* 仓库
    * 构建私有仓库


## 安装

* jenkins  OK

* mysql

```docker run -d -p 13306:3306  --name mysql2 -v /opt/mydata/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:latest```


* Gogs

```docker run --name=mygogs -p 10022:22 -p 10080:3000 -v /var/gogs:/data gogs/gogs```