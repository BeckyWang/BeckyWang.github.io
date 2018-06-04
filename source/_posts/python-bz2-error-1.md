---
title: No module named bz2错误解决方法
date: 2018-04-17 17:19:15
tags: [python]
---
最近在学机器学习，想跑一些demo，运行过程中报的错。

	ModuleNotFoundError: No module named '_bz2'

这说明python在运行的时候少了一个_bz2的包。
先后试了pip安装，但是都失败了，提示没有这样的包。

	pip3 install _bz2
	pip3 install bz2


上网查了一下，\_biz主要提供了支持bzip2压缩算法的操作功能。
如果python是你自己编译的，实际上需要安装bzip2-devel这个包后再编译，否则bz2无法使用。

按照网上的方法，实际操作了一遍，解决了问题。

1. 重装bzip2。

		wget http://www.bzip.org/1.0.6/bzip2-1.0.6.tar.gz
		tar -zxf  bzip2-1.0.6.tar.gz 

		cd bzip2-1.0.6  

		make -f  Makefile-libbz2_so 

		make && make install
	
2. 验证是否安装成功
进入python3环境，输入import bz2，回车，若没有报错即成功。

更多信息参考[这个网站](https://my.oschina.net/yangertt2006/blog/839220)
