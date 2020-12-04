# Git 常常提到的操作还有:

**git cherry-pick** 挑选所需提交合并

**git stash** 将修改的内容暂存至堆栈区

**git rebase -i** 交互式 rebase，修改一些已经提交的修改

**git push origin :another_branch** 删除远程仓库的 another_branch 分支（将一个空分支 push

到远程仓库的 another_branch 分支）

**git fetch:**

git fetch 没有任何参数的时候，默认会下载**所有**的远程仓库提交的记录，也就把本地仓库的所有 **origin/**远程分支更新到最新的版本。它不会去修改我们本地的任何分支，也不会修改本地的工作区间的任何文件，所以非常安全。Git fetch 最常使用的场景就是我们要提交本地修改到远程仓库的时候，也是比较**推荐的规范**： 

**（1） 先** **git fetch** **下载别人的修改，**

**（2） 然后** **git diff master origin/master** **对比下本地** **master** **修改和别人修改的不同**

**（3）** **再** **git megre origin/master** **或者** **git rebase origin/master** **自己和别人的提交，**

**（4）** **最后** **push** **到远程仓库。**如果只想取回特定远程分支的更新 git fetch origin master。

想要创建个同名的本地分支和该远程分支关联，git checkout new_branch

如果不同名 git checkout  -b 本地分支名 远程分支名

如果不需要切换到该分支 git branch 本地分支名 远程分支名

(**可以通过 git branch –vv 或者 git remote show orgin 查看本地分支和远程分支的对应关**

**系**)

如果本地分支已经存在了

git branch --set-upstream-to=origin/new_branch new_branch等同于 git branch -uorigin/newbranch newbranch 即可关联起来该本地分支和指定的远程分支

git push --set-upstream origin new_branch 推送本地新建的分支到远程仓库

**git merge 和 git rebase 合并分支：**

git merge 就是现在做的一样，先pull下来，合并，push

git rebase 是创建本地分支的复制提交跟在远程仓库commit的后面。（也要pull下来，合并）合并结果不同

![Git merge](../img/Git/git%20merge.png)

![Git rebase](../img/Git/git%20rebase.png)





git diff 工作区和暂存区差异，也就是即将 add 的文件（注意新增的文件不会显示）

git diff --cached 暂存区和本地仓库的差异，也就是即将 commit 的文件

git diff HEAD 工作区和本地仓库 HEAD 指向的差异

git diff SHA1 SHA2 比较两个历史版本的差异

**git log --graph --all** 也是个非常常用的指令，查看各个分支的提交树信息

**git commit --amend，**追加提交



**git show 也是个常用的指令，查看某次提交修改内容**

**git status 是个常用指令**，可以查看工作区（哪些文件改动了）和暂存区（哪些文件处

于待提交）的状态。

git add 添加到暂存区 + git commit 添加到本地仓库  = git commit -a 对新创建文件无效



#### 回退

**git reset --hard HEAD** ：重置暂存区和工作目录（没有commit过的都删除）

在不要本地修改时，配合使用git clean -df 删除当前目录下没有被 track 过的文件和文件夹，安全第一可以先 git clean -ndf 查看哪些文件将被删除

**git reset --mixed**:（git reset不带参，默认）保留工作目录，并清空暂存区

git reset --hard HEAD 可以回退所有修改，如果文件未添加到暂存区也可以通过 git checkout -- file 方便的回退工作区修改（不加--貌似也可以正常使用，整个目录 git checkout ）。这里的回退有个优先级，如果暂存区有则会回退到暂存区状态，否则会回退到本地版本的状态

git reset 一般是还未推开到远程分支的时候使用。当已经推到了远程仓库了，我们一般使用 git revert 来回退我们某些不需要的修改，因为使用 git reset 的话就需要添加-f 参数，并不推荐。git revert HEAD 回退最近一次的修

改，git revert commit-hash 回退某次具体提交 id 的修改。revert 操作是以新的一次 commit提交的方式来回退某次改动，会产生新的提交记录，而 reset 则是直接删除掉某些提交记录。

**git reset –-soft HEAD**:保留工作目录，并把重置 HEAD 所带来的新的差异放进暂存区

（工作区和HEAD不同的放入暂存区）

就是本地仓库回退到HAED，修改存入add后暂存区，本地工作区不变

#### 创建

```csharp
//添加远程仓库地址
git remote add origin https://....git //origin 是该远程仓库在本地的别名，可自定义。
//添加所有文件到缓存
git add .
//提交到本地仓库
git commit -m "first commit"
//推送到远程仓库
git push origin master //master是分支名称
git clone url //copy code
```

#### 子仓库

添加子仓库

git submodule add url

git commit -m 提交信息

git push

递归拉取仓库

git clone URL

git submodule update --init --recursive

git submodule foreach git checkout master

git submodule foreach git submodule update --init

如何切换所有的子仓库到指定主分支 

git submodule foreach git checkout master

常用的还有如何更新所有子仓库 

git submodule foreach git pull origin master

参考：

https://blog.csdn.net/wh_19910525/article/details/7554489

