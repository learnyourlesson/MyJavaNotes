## git 放弃本地修改，强制拉取更新

```git
git fetch --all 
git reset --hard origin/master 
git pull //可以省略
```

git fetch 指令是下载远程仓库最新内容，不做合并
git reset 指令把HEAD指向master最新版本

## git强制推送命令

```
git push -f origin [localBranch]:[remote]
```

