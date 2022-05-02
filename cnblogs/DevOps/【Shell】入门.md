## Shell

> C 语言编写的程序  
> 命令语言  
> 程序设计语言  
> 操作系统内核

* sh :Unix Shell
* Windows Explorer :图形界面 Shell

## Shell 环境

> Bash/Bourne Again Shell  
> Bash 也是大多数Linux 系统默认的 Shell

```shell
#!/bin/bash
echo "Hello World !"
```

```#!``` 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell

## shell 命令

* echo 字符串的输出 会自动添加换行符
* printf 比echo移植性好 换行需要添加```\n```

## test

> 用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试


## 流程控制

### if

```shell
if condition
then
    command1
    command2
    command3
    ...
    commandn
elif condition2
then
    command22
else
    commande
fi
```

### for

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

### while

```shell

while condition
do
    command
done

```

### 无限循环

```shell
while :
do
    command
done

# 或者
while true
do
    command
done

# 或者
for (( ; ; ))

```

### until 循环

```shell
until condition
do
    command
done


```


