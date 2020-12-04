# Git stash

**git stash**									缓存当前的未提交（包括暂存的和非暂存的（add））修改

**git stash pop[–index] [stash_id]**		弹出（会删除）最新（默认）[stash_id]的缓存，[-index] 会尝试把暂存区的放回暂存区，默认是都放在工作区

**git stash save {message}**		给缓存加上缓存信息

**git stash list**								显示保存进度的列表

**git stash drop [stash_id]**		删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

**git stash clear**							删除所有存储的进度。

**git stash show[-p] [stash_id]**			查看指定stash的diff（默认最新）,[-p]查看全部diff

**git stash branch**						从缓存中创建新分支，如果成功清除缓存

**git stash apply [–index] [stash_id]**		除了不删除恢复的进度之外，其余和git stash pop 命令一样

默认会忽略：

1. 新文件（untracked files）
2. 被忽略的文件（ignored files）

可以使用 git stash [-u]/[-a]  [-u]缓存新文件 [-a]缓存全部修改



参考：https://blog.csdn.net/daguanjia11/article/details/73810577

​			https://www.cnblogs.com/tocy/p/git-stash-reference.html

​			官方文档