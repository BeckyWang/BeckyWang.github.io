---
title: git常用命令——本地仓库
date: 2018-06-02 16:32:40
tags: [git]
---
最近在学习git的使用，总结于[廖雪峰的网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 创建一个版本库
$ mkdir &lt;repository-name&gt;
$ cd &lt;repository-name&gt;
$ pwd 显示当前目录

p.s. 所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

## 本地仓库git常用命令
### 初始化
**git init** 初始化一个git仓库

**git add &lt;file&gt;** 暂存文件到暂存区
**git add .** 暂存所有文件

**git commit  -m '\*\*\*'** 提交文件到当前分支（本地仓库）

p.s. 建Git版本库时，Git自动为我们创建了唯一一个master分支，所以git commit就是往master分支上提交更改。
每次修改，如果不用git add到暂存区，那就不会加到commit中。

### 查看状态
**git status** 查看仓库当前的状态

**git diff** 查看difference

### 版本回退
**git log** 显示从最近到最远的提交
**git log --pretty=online** 简化输出信息

**git reset --hard HEAD^** 回退到上一个版本
**git reset --hard &lt;commd-id&gt;** 回到指定版本，版本号写几位就行

**git reflog** 记录每一次命令

p.s. $ cat &lt;file&gt; 查看文件详细信息

### 撤销修改
**git checkout -- &lt;file&gt;** 丢弃工作区的修改，即让文件回到最近一次git commit或git add时的状态（没有--，就变成了“切换到另一个分支”的命令）

**git reset HEAD &lt;file&gt;** 把暂存区的修改撤销掉，重新放回工作区（HEAD表示最新的版本）

### 删除文件
**$ rm &lt;file&gt;** 本地（工作区）删除文件

**git rm &lt;file&gt;** + **git commit**
从版本库（本地仓库）删除文件，然后提交
