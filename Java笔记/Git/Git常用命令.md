## 恢复暂存区的指定文件到工作区
$ git checkout [file]

## 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

## 恢复暂存区的所有文件到工作区
$ git checkout .

## 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

## 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

## 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

## 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop