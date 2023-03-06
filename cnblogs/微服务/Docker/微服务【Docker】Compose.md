## Compose

定义和运行多容器 Docker 应用程序的工具

使用 YML 文件来配置应用程序需要的所有服务

1. 安装docker-compose

  ```shell
  $ sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

  # 测试安装成功
  $ docker-compose version
  ```


2. 配置```docker-compose.yml```

3. 构建运行

  ```shell
  docker-compose up
  # 后台运行
  docker-compose up -d
  ```


