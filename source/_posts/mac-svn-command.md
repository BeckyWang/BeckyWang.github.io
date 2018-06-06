---
title: mac终端下svn常用命令
date: 2018-05-31 15:42:01
tags: [svn]
---
### checkout
**svn checkout &lt;url&gt;** 将文件checkout到本地，url是服务器上的目录
**svn co &lt;url&gt;** 简写

### 添加文件
**svn add &lt;file&gt;** 将本地新文件添加到版本库
**svn add \*.js** 添加当前目录下所有js文件

### 提交代码
**svn commit -m '\*\*\*'** 提交改动的文件到版本库
**svn ci -m '\*\*\*'** 简写
**svn ci &lt;path&gt; -m '\*\*\*'** 只提交某个路径下的文件

### 更新文件
**svn update -r m &lt;path&gt;** 将某个路径下的本地文件还原到版本m
**svn update -r 200 test.js** 将本地的文件test.js还原到版本200
**svn update** 如果后面没有目录，默认将当前目录以及子目录下的所有文件都更新到最新版本。
**svn up** 简写

### 查看文件或目录状态
**svn status &lt;path&gt;** 查看目录下的文件和子目录的状态，正常状态不显示（?：不在svn的控制中；M：内容被修改；C：发生冲突；A：预定加入到版本库；K：被锁定）
**svn status -v &lt;path&gt;** 显示文件和子目录状态（第一列保持相同，第二列显示工作版本号，第三和第四列显示最后一次修改的版本号和修改人）
**svn st** 简写

### 创建纳入版本控制的新目录
**svn mkdir $URL/dir -m '\*\*\*'** 工作空间或者版本库创建目录，产生提交

### 删除文件
**svn delete &lt;path&gt; -m '\*\*\*'** 删除某个路径下的文件并提交
相当于：
**svn delete &lt;path&gt;**
**svn ci -m '\*\*\*'**

**svn mv &lt;path_old&gt; &lt;path_new&gt; -m '\*\*\*'** 【mv=move  等价于svn cp 后svn delete】移动或拷贝工作空间或者版本库的文件/目录，也可用于文件改名

**svn rm &lt;path&gt; -m '\*\*\*'** 目录删除

### 查看信息
**svn help &lt;command&gt;** 获取子命令说明

**svn log &lt;path&gt;**  查看文件所有修改记录及版本号的变化
**svn log m:n** 查看版本m到n的历史信息

**svn info &lt;path&gt;** 查看文件详细信息

**svn list** 显示给定目录在某一版本存在的文件（显示当前目录下svn记录文件列表，不访问版本库）
**svn list &lt;url&gt;** 不下载到本地查看目录中的文件

**svn cat** 在屏幕打印某个文件特定版本内容
**svn cat -r m &lt;file&gt;** 显示文件指定版本内容

### 拷贝
**svn cp $URL1 $URL2 -m '\*\*\*'** 工作拷贝或者版本库之间文件的相互拷贝
工作拷贝  -> 工作拷贝  ：  复制和通过调度进行增加(包含历史)
工作拷贝  -> 地址(URL) ： 马上提交一个工作拷贝到地址(URL)
地址(URL) ->工作拷贝  ：  签出地址(URL)到工作目录，通过调度进行增加
**地址(URL) ->地址(URL)** ： 完全服务器端复制；一般用于**分支**和**标签**

### 比较差异
**svn diff &lt;path&gt;** 将修改的文件与基础版本比较
**svn diff -r m &lt;file&gt;** 比较本地工作拷贝与版本库指定版本
**svn diff -r m:n &lt;url&gt;** 对版本m和版本n比较差异
**svn di** 简写

### 合并
**svn merge -r m:n &lt;path&gt;** 从版本记录m和n之间的变化合并到当前工作目录中。
合并完成后再提交（ci）

### 版本回退
**svn merge -r n:m &lt;path&gt;**
从高版本到低版本就是回退，从低版本到高版本增加记录。

### 恢复本地修改
**svn revert &lt;path&gt;** 恢复对文件或者目录的修改，用于未执行提交操作(ci)之前，撤销本地修改。【本地操作，会丢失修改，慎用】
本子命令不会存取网络，并且会解除冲突的状况。但是它不会恢复被删除的目录.

p.s. merge是已提交的代码回退，revert是没有提交的代码回退。

### 解决冲突
**svn resolved &lt;file&gt;** 删除冲突标记，这条命令不会依语法来解决冲突或是移除冲突标记；它只是移除冲突的相关文件，然后让file可以再次提交。

### 切换分支
**svn switch URL** 更新工作副本至不同的URL
**switch URL\[@PEGREV\] \[PATH\]** 更新工作副本，切换到同一版本库中的新 URL。其行为跟“svn update”很像，也是将工作副本切换到同一版本库中某个分支或者标签的方法。PEGREV 决定从哪个版本查找目标。
**switch –relocate FROM TO \[PATH\]** 重写工作副本的 URL 元数据，以反映单纯的 URL 改变。当版本库的根 URL 改变(比如方案或者主机名称变动)，但是工作副本仍旧对应同一版本库的同一目录时，使用这个命令更新工作副本与档案库的对应关系。

p.s. switch命令更多参考[这里](https://blog.csdn.net/jk110333/article/details/9301283)

