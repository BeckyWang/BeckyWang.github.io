---
title: 今天你记仇了吗
date: 2018-05-22 16:23:33
tags: html
---
浏览掘金时发现一个很好玩的表情包生成器，也是最近比较火的表情包：今天这个仇先记下来了。
<img src="/images/jichou.png" alt="jichou" style="width: 300px">

原理非常简单，具体实现方式就是利用截图工具 [html2canvas](http://html2canvas.hertzen.com/) 对一个 Div 进行截图，在这个 Div 里有记仇的图片和可编辑的文本框，文字里的时间自动生成。

亲自实现了下，还是挺有意思的，下次需要自定义记仇表情包就可以直接生成了。

点击 [这里](https://github.com/BeckyWang/hold-the-grudge) 查看代码。