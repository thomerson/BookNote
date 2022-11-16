## 复制某个分支的某个commit提交到其他分支上去

```
git log --找到某个commit的id
git checkout --切换到要提交的分支

git cherry-pick id --复制提交到当前分支

git push

```


## 撤销commit

```
git reset --soft head^  --上一个版本
git reset --soft head~1  --后面的数字表示撤销最近的几次提交
```

```--soft``` 不删除工作空间改动代码，撤销commit，**不撤销git add**

```--mixed``` 不删除工作空间改动代码，撤销commit，**并且撤销git add**

```--hard``` **删除工作空间改动代码**，撤销commit，撤销git add

## git ignore

已经在git记录中的文件，添加整个的```git.ignore```来忽略文件例如bin和obj路径下的编译文件


修改后没有立即生效，需要清理cache

```shell
git rm -r --cached .

git add .

```

然后commit就OK了

