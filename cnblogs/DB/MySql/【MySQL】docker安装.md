## docker安装

```powershell

docker pull mysql:8.0

docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql:8.0

# 持久化数据
docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=123456 -v /path/on/host:/var/lib/mysql -p 3306:3306 mysql:8.0

```
