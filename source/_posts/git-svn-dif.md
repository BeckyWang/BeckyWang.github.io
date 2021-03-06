---
title: git和svn的区别
date: 2018-05-30 10:21:24
tags: [git, svn]
---
1. git是分布式的，svn是集中式的。
	git跟svn一样也有自己的集中式版本库或服务器。但git更倾向于被使用于分布式模式，也就是每个开发人员从中心版本库/服务器上chect out代码后会在自己的机器上克隆一个自己的版本库。

2. git把内容按元数据方式存储，svn按文件方式。
	.git目录是处于你的机器上的一个克隆版的版本库，它拥有中心版本库上所有的东西，例如标签，分支，版本记录等。

3. git分支比svn分支强大。
	svn的分支就是一个完整的目录，且这个目录有完整的实际文件。若创建一个分支，每个工作成员都会有一份一模一样的新分支。
	git上，每个工作成员可以任意在自己的本地版本库开启无限个分支。只要不合并及提交到主要版本库，没有一个工作成员会被影响。

4. git没有一个全局的版本号，svn有。

5. git的内容完整性要优于svn。
	git的内容存储使用的是SHA-1哈希算法，这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。
	另外在git数据库中的东西都是用此哈希值来作索引，而不是靠文件名。

6. git下载下来后，不用联网即可在本地查看所有log，svn需要联网。
	因为git上保存着所有当前项目的历史更新，直接从本地数据库读取后展示给你看。

7. svn断开网络或者断开VPN就无法commit代码，但是git可以先commit到本地仓库。
	因为svn每次commit都必须联网，长时间不commit代码会丢失大量开发进程的历史纪录。
	git可以频繁提交到本地仓库，且不需要网络，速度非常快，等到有网时，再将版本纪录和代码一起上传到远程仓库。

参考网站 
<https://blog.csdn.net/Peter_tang6/article/details/76577108>
<https://blog.csdn.net/hellow__world/article/details/72529022>