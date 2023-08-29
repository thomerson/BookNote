## 原因


## 步骤

### mysqld服务跳过验证授权表开启

1. 管理员身份运行cmd，cd到mysql的bin目录


2. 关闭mysql服务

```shell
net stop mysql
```

3. 在cmd窗口中开启mysqld服务，其登录后不验证授权表


```shell
# 开启跳过授权表的mysqld服务
mysqld --console --skip-grant-tables --shared-memory
```

### 使用root账户连接数据库并重置密码为空

4. 当前不关闭，另开一个cmd，重复步骤1


5. 使用root账户登录mysql，不验证密码

```shell
mysql -u root -p
```

6. 在mysql数据库user数据表中root账户重置密码为空

```shell

use mysql

update user set authentication_string='' where user='root';
```

7. quit命令关闭mysqld服务并关闭窗口，关闭root账户登录窗口


### root账户密码由空进行修改

8. 重开窗口开启mysql服务

9. 重开窗口使用root账户连接数据库

```shell
mysql -u root -p
```

<!-- TODO 没有成功，先用虚拟机的， 有时间再弄-->



