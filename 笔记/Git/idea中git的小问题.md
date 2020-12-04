idea使用git提交，不知道为什么没有提交我当前的项目，而是提交了之前的项目，幸好没啥问题，只是多了个分支，删掉就好

```
git branch -r 查看分支
```

```
git branch -r -d 分支名
```

```
git push origin :分支名(冒号前面有个空格)
```

第二句删除本地分支

第三句推送结构到远程

例子：

```
git branch -r -d origin/branch-name git push origin :branch-name   删除branch-name 分支
```

切换分支：

```
git checkout [branch-name]
```

