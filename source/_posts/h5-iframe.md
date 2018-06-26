---
title: H5页面PC富文本内容自适应显示
date: 2018-06-25 11:11:41
tags: [html, 富文本, iframe]
---
接[上篇博客](https://beckywang.github.io/h5-damping-effect.html#more)，现在增加一个新需求：h5页面B区显示富文本HTML片段。

## 功能描述
* 要求

	* 有一段PC端显示的富文本HTML片段，在手机H5页面B区上加载显示
	* 保持PC端的样式缩放适应手机屏幕
	* 如果HTML富文本有图片

		* 图片默认不加载
		* 当手机可视区间到B区时候，图片触发懒加载显示
		* 点击富文本图时候，有弹层加载图片，可放大查看
* 提示

	* 可以考虑用iframe方式处理富文本
	* 但是要考虑到iframe的父子页面的通信和兼容性
	* 还有考虑到iframe里富文本图片懒加载导致高度变化带来的父页面的高度自适应

## 具体实现过程
### 为什么选择iframe
看到需求时，第一想到的是只是显示一段富文本内容，为什么要用iframe，直接显示不就行了。
iframe还是有挺多缺点的：
1. 会产生很多页面，不容易管理
2. 不容易打印
3. 浏览器的后退按钮无效
4. 代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理iframe中的内容，所以使用iframe会不利于搜索引擎优化。
5. 多数小型的移动设备（PDA 手机）无法完全显示框架，设备兼容性差。
6. 多框架的页面会增加服务器的http请求，对于大型网站是不可取的。

这么多缺点为什么还要用呢（关键是我对iframe的使用不太熟悉，手动微笑。。。）
高人指点，因为想用iframe隔离全局样式对富文本样式的影响，emmmm，确实很有道理。

比如你在全局样式里写了一个样式：
				
		p {
			font-size: 18px; 
		}
那么富文本的内容也会受到这个全局样式的影响，所有p里面的字体大小都会是18px。这可不是我们想得到的。

其实看到这，肯定会想到，只要在全局样式里注意一下，不要写一些可能会影响全局的样式就行了，比如不写标签的样式。这对于一个新项目可能较为容易去实现，如果一个项目已经非常庞大或经过多人的修改，把不规范的样式全部改掉好像有点不太现实。

好了，暂时没有想到其他好的办法，只能选择iframe。

### 实现思路
1. 图片自适应
因为返回的数据是一段html片段，所以选择iframe的srcdoc属性（规定在&lt;iframe&gt;中显示的页面的 HTML 内容），我们可以在获取到数据之后对数据进行一些处理（图片懒加载，后面会讲到），然后再字符串拼接成一段完整的HTML。

html中设置meta标签，允许用户缩放：

	<meta name="viewport" content="width=device-width, initial-scale=1">
设置全局样式

	<style>
        body {height: ${clientHeight}px;}   
        img {max-width: ${clientWidth}px;}
    </style>
clientHeight是屏幕高度，clientWidth是屏幕宽度。
设置body的高度是因为这篇博客需求是接着上篇博客的需求来的（需要实现阻尼效果），在iframe里，我禁用了scroll，具体思路可参考[上篇博客](https://beckywang.github.io/h5-damping-effect.html#more)。
设置img最大宽度是为了实现图片自适应，不会因为太大而超出屏幕。

2. 图片懒加载
直接上代码：
修改字符串内容：

		const newHtml = result.content.replace(/<img.*?\/>/g, match => {
	        return match.replace('src', 'src="./images/iframe/loading.gif" data-src');
	    });
懒加载函数：

		const clientHeight = $(window).height();
	    let continueLoading = true;
	    const imgTags = document.getElementsByTagName('img');
	    const lazyload = () => {
	        let num = 0;
	        for (let img of imgTags) {
	            const { top } = img.getBoundingClientRect();
	            if (/loading.gif/.test(img.src)) {
	            	top < clientHeight && (img.src = img.dataset.src);
	            } else {
	            	num++;
	            }
	        }
	        num === imgTags.length && (continueLoading = false);
	    };

	    $('.main').on('touchmove', e => {
	        continueLoading && lazyload();
	    });

我这里是先用正则进行替换，先显示loading，并用data-src记住图片地址。然后再touchmove事件中检测并进行懒加载：
getBoundingClientRect()用来检测图片是否在可视区域内。
continueLoading是一个标志，当所有图片都加载完成后，就不再触发lazyload函数。

3. 图片弹层查看
这个非常简单了，用一个绝对定位的div去显示图片就可以了，div背景色设置成白色（或者是其他你喜欢的颜色），display: none，当点击iframe中的图片时，设置div里img标签src的值，并显示div。再次点击弹层的图片，隐藏弹层。

		$('.main img').on('click', e => {
	        $('.img-modal .img-detail').attr('src', e.target.src);
	        $('.img-modal').show();
	    });
	    $('.img-modal').on('click', () => {
        	$('.img-modal').hide();
        });

3. 父子间通信
因为在ifame所在的B区也要实现阻尼效果。其他部分的思路和A区的阻尼效果实现思路相同，只不过当用户到达页面顶部并下拉到临界值时，使用postmessage通知父页面显示A区，隐藏B区。
iframe中:
		
		$('.main').on('touchend', e => {
        	//其他逻辑
        	...
	        if (last_distance >= RANGE_TOP) {
	            //切换到上一个页面
	            window.parent.postMessage('prev', '*');
	            ...
	        }
	    });
父页面中：

		window.addEventListener('message', rs => {
	        if (rs.data === 'prev') {
	            $('.pageB').hide();
	            $('.pageA').show();
	            ...
	        }
	    });

## 最终效果
![效果](h5-iframe/last.GIF)

## 优化
代码实现的较为粗糙，还有很多可以优化提高的地方。
- 阻尼效果：AB区实现思路一模一样，可考虑抽成公共插件。
- 图片点击：使用fastclick或zepto对移动端的click事件进行优化。

具体代码见[我的github](https://github.com/BeckyWang/h5-damping-effect)，并详细介绍了项目的使用方法。