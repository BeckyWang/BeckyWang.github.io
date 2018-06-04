---
title: git常用命令——远程仓库
date: 2018-05-05 16:42:23
tags: [git]
---
参考 [廖雪峰的网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 和 [这个网站](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5)

## 连接本地仓库和远程仓库

- 创建SSH $ ssh-keygen -t rsa -C "email-address"
- 登录远程仓库（github），添加 SSH KEY：在Key文本框里粘贴id_rsa.pub文件的内容。


p.s. GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能送。
GitHub允许你添加多个Key。只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

## 远程仓库git常用命令
### 查看当前远程库
**git remote -v** -v及--verbose缩写，显示对应的克隆地址

### 添加远程仓库
**git clone &lt;address&gt;** 在本地克隆远程仓库

**git remote add origin &lt;address&gt;** 将本地库和远程库关联。（远程库的名字就是origin，这是Git默认的叫法，也可以改成别的）

**git fetch &lt;remote-name&gt;** 从远程仓库抓取数据到本地（remote-name指远程库对应的名字）

p.s. 此命令会到远程仓库中拉取所有你本地仓库中还没有的数据。运行完成后，你就可以在本地访问该远程仓库中的所有分支，将其中某个分支合并到本地，或者只是取出某个分支，一探究竟。
如果是克隆了一个仓库，此命令会自动将远程仓库归于 origin 名下。所以，git fetch origin 会抓取从你上次克隆以来别人上传到此远程仓库中的所有更新（或是上次 fetch 以来别人提交的更新）。

p.s. fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并。
 git pull目的是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。

### 推送数据
**git push -u \[remote-name: origin\] \[branch-name: master\]** 把本地库的所有内容推送到远程库（把本地的 master 分支推送到 origin 服务器上）（加上-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。）

### 远程仓库管理
**git remote show \[remote-name\]** 查看某个远程仓库的详细信息

**git remote rename &lt;old_name&gt; &lt;new_name&gt;** 修改某个远程仓库在本地的简称（对远程仓库的重命名，也会使对应的分支名称发生变化）

**git remote rm \[remote-name\]** 移除对应的远端仓库

