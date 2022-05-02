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


### case

```shell
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2)
    command1
    command2
    ...
    commandN
    ;;
esac
```

### 跳出循环

* break

* continue

## 函数

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73

```