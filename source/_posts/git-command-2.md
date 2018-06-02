---
title: git常用命令——远程仓库
date: 2018-06-02 16:42:23
tags: [git]
---
最近在学习git的使用，总结于[廖雪峰的网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 远程仓库git常用命令
### 连接本地仓库和远程仓库

- 创建SSH $ ssh-keygen -t rsa -C "email-address"
- 登录远程仓库（github），添加 SSH KEY：在Key文本框里粘贴id_rsa.pub文件的内容。


p.s. GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能送。
GitHub允许你添加多个Key。只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

### 把本地仓库的内容推送到GitHub仓库
git remote add origin &lt;address&gt; 将本地库和远程库关联。（远程库的名字就是origin，这是Git默认的叫法，也可以改成别的）

git push -u origin master 把本地库的所有内容推送到远程库（加上-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。）

### 克隆仓库
git clone &lt;address&gt; 在本地克隆远程仓库
