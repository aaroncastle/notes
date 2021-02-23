# Git

`git remote add [xxx] git@github.com:xxx/xxx.git` 为远程地址换成一个新名字

## 查看版本号

```shell
git log
git log --oneline
git log --oneline --decorate #显示分支
git log --oneline --decorate --graph #图形化展示
```

## 回退版本

```shell
git reset --hard [hash] #回退后再用 git log只能查看当前版本与之前的版本号，但原来的版本号不再显示
```

## 查看原来的历史记录

```shell
git reflog
```

## git 合并

```shell
git merge [分支名]
```

## 删除分支

```shell
git branch -d [分支名] #要退出要删除的分支
```

## 将文件状态暂时存储

```shell
git stash #一般用来做切换分支前的操作。现在修改的内容还不至于commit一次，所以暂时保存一次。之后就可能用来切换分支了。可以stash多次。因为存储方式是栈存储，所以stash@{0}永远是最近的一次保存。
```

## 查看临时存储有哪些

```shell
git stash list
```



## 取出保存的内容

```shell
git stash pop #取出最近的一次存储并删除。取出后类似于用数组的pop方法删除
git stash apply stash@{number} # 取出后并不会删除保存
git stash drop stash@{0} # 删除一个存储
```

## 变基

（结果与merge一致，但提交线将只有一条，更干净）

```shell
git rebase --onto [newbase] [start] [branch] #作用是在改变的分支上把改变的提交移植到 [newbase]分支上，然后branch在base线的最前端。此改变必须得在[branch]的分支上来做。
# --onto [newbase]的缺省为与start一致。[branch]的缺省为HEAD
git merge [branch] #利用master的快进式merge。将整个分支变成一条线。
```

## 远程分支 （origin remote)

```shell
git branch #只能查看本地分支
git branch -a #可以看到本地与远程分支
```

## git fetch 与 git pull的区别

git fetch 只获取远程的分支，但不merge，需要手动merge

```shell
git fetch
git merge origin/master
```

 `git pull`相当于`get fetch`后又`git merge origin/master`，快进式的merge。相当于以上两步。

## git 修改上一次的提交说明

```shell
git commit --amend -m 'xxxx'
```

## git 撤消上一次提交

会覆盖暂存区不会覆盖工作区。回退版本`git reset --hard [hash]`会覆盖工作区

```shell
git reset --mixed master^ # master^代表上一次的master分支的hash
```

## git add的回退操作

```shell
git -- .
```

## git commit的修改

仍然保存了内容，并且hash不会变

```shell
git reset --soft master^
```

<font style='color: orange'> git reset 默认选项是 --mixed</font>

## cherry-pick 

将任意提交的修改应用于HEAD分支

```shell
git cherry-pick [hash]
```

