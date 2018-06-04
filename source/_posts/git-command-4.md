---
title: git常用命令——分支管理
date: 2018-06-04 09:28:03
tags: [git]
---
更多参考 [这个网站](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF)

## 创建合并分支
**git checkout -b dev** 创建dev分支，然后切换到dev分支（-b表示创建并切换），相当于：
**git branch dev** 
**git checkout dev**

**git branch** 查看当前分支（当前在dev上）
**git branch -v** 查看各个分支最后一个提交对象的信息
**git branch --merged** 查看哪些分支已被并入当前分支（也就是说哪些分支是当前分支的直接上游，所以列出来且没有\*的分支可以直接删除）
**git branch --no-merged** 查看尚未合并的工作（由于这些分支中还包含着尚未合并进来的工作成果，所以简单地用 git branch -d 删除该分支会提示错误，因为那样做会丢失数据，可以用大写的删除选项 -D 强制执行）

**git checkout master** 切换分支（切换到master上）

**git merge dev** 合并指定分支到当前分支（把dev分支的工作成果合并到master分支上，Fast-forward模式）

**git branch -d dev** 删除dev分支

**git log --graph** 查看分支合并图

p.s. 解决冲突：当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

**git merge --no-ff -m "merge with no-ff" dev** 以禁用ff的模式合并dev分支（本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。）

p.s. 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

2.远程分支
远程分支（remote branch）是对远程仓库中的分支的索引。
它们是一些无法移动的本地分支；只有在 Git 进行网络交互时才会更新。
远程分支就像是书签，提醒着你上次连接远程仓库时上面各分支的位置。

用 (远程仓库名)/(分支名) 这样的形式表示远程分支。

**git push <远程主机名> <本地分支名>:<远程分支名>** 推送本地分支
git push origin serverfix 取出我在本地的 serverfix 分支，推送到远程仓库的 serverfix 分支中去

p.s. 对于无意分享的分支，你尽管保留为私人分支好了，而只推送那些协同工作要用到的特性分支.
推送本地分之后，当你的协作者再次从服务器上获取数据时（fetch），他们将得到一个新的远程分支 origin/serverfix，并指向服务器上 serverfix 所指向的版本。
在 fetch 操作下载好新的远程分支之后，你仍然无法在本地编辑该远程仓库中的分支。换句话说，在本例中，你不会有一个新的 serverfix 分支，有的只是一个你无法移动的 origin/serverfix 指针。

**git checkout -b serverfix origin/serverfix** 切换到新建的 serverfix 本地分支，其内容同远程分支 origin/serverfix 一致

p.s. 上述操作会在远程分支的基础上分化出一个新的分支。
如果要把该远程分支的内容合并到当前分支，可以运行 git merge origin/serverfix。

从远程分支 checkout 出来的本地分支，称为 跟踪分支 (tracking branch)。跟踪分支是一种和某个远程分支有直接联系的本地分支。在跟踪分支里输入 git push，Git 会自行推断应该向哪个服务器的哪个分支推送数据。同样，在这些分支里运行 git pull 会获取所有远程索引，并把它们的数据都合并到本地分支中来。

**git checkout --track origin/serverfix** 跟踪远程分支（同上一条命令），简化版本（git 1.6.2以上版本）

**git push <远程主机名> :<远程分支名>** 删除远程分支

**git branch --set-upstream &lt;branch-name&gt; origin/&lt;branch-name&gt;**  建立本地分支和远程分支的关联

3.分支的变基
p.s. rebase操作可以把本地未push的分叉提交历史整理成直线。
使用变基的目的，是想要得到一个能在远程分支上干净应用的补丁。

**git checkout a_branch** 
**git rebase b_branch** a_branch是要进行变基的分支，b_branch是基地分支，即改写a_branch的提交历史，使它成为b_branch的直接下游。

**git rebase \[主分支-基底\] \[特性分支\]** 将特性分支变基到主分支上

**p.s. 一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行变基操作。**
