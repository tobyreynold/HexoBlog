---
title: 如何用Nodejs写一些小工具
date: 2016-06-19 14:06:26
tags: Nodejs
---

## 引言

6月初最近没怎么写文章👿，不是因为偷懒，是因为端午节前去了趟上海迪士尼，回来之后新岗位事情相对较多，在公司内部搭了个MkDoc花了一些业余时间，然后欧洲杯又开幕了😄，看⚽必须是️优先的啊，所以没怎么写。不过话说上海迪士尼虽然没全部建完，不是那么大，但是真心值得一去，从早上8：00开园就进去了，一直到看完🎆才撤退，虽然累成🐶，但是真心好玩呀，大项一定记得要拿pass卡，这样才能保证整个游玩的体验😄，越写越跑题，下面赶紧进入正题😢：
网上有各种各样基于Nodejs的工具，不管还是靠谱不靠谱的，总之就是又多又乱，形形色色，五花八门，也有很多雷人的，雷人的就不说了哈😄。工作中其实还是做业务的童鞋比较多，对怎么下手写一个Nodejs的工具包还不是特别有思路，其实当你有了思路之后发现也挺容易的，下面就针对一个简单case，稍微讲一下怎样从一个很简单🌰入手。

## 正题

平常在工作中也有可能遇到这样的情况，比如设计师把设计稿切完给你很多图片（当然很多公司得前端自己切😄），然后图片文名起的不是很满足前端的命名规范，很多图片名字用下划线连接的，例如:"icon_star"，但是我们想把它改成"icon-star"，或者其他的命名方式，但是文件有好几十个，那么这时候手工改几十个文件起来就实在太费劲了，其实这时候用Nodejs写一个package就很容易把这个问题用技术的方法更好的解决掉，而且Nodejs的自带的fs模块对于文件的流操作有十分擅长。

npm如何发布一个package，请移步我之前写的，[npm必须掌握的基础概念](https://codefilled.com/2016/05/16/npm%E5%BF%85%E9%A1%BB%E6%8E%8C%E6%8F%A1%E7%9A%84%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5/)，这里就不过多讲解了，主要讲一下如何实现上面的那个case。

## 代码实现

在写这个工具前，可以异步Nodejs的官方文档，熟悉一下fs模块，链接:[fs模块文档](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html)，其实可以看到里面提供了很多的API供我们使用。
实现上面的这个case其实就会用到两个主要的API就够了，一个是fs.readdir，另外一个是fs.rename，看这两个名字就大概清楚是做什么的了，一个是读取某个目录，另外一个是给文件改名字。下面是处理这个问题的核心代码：

``` bash
$vim {project}/index.js
var fs = require('fs');
var src = 'file'; /* project/file 下面的需要修改名字的文件目录 */
fs.readdir(src,function(err,files) {
	console.log(files);
	files.forEach(function(filename) {
		var oldPath = src + '/' + filename;
		var newPath = src + '/' + filename.replace(/_/g,"-");
		fs.rename(oldPath,newPath,function(err) {
			if(!err) {
				console.log(filename + "替换下划线成功！");
			}
		});
	});
});
$node index.js
``` 

执行node index.js之后，就可以看见结果了。

``` bash
/* 本机执行的结果，上面代码console打出来的log */
[ 'icon_font.js','npm_pkg.js','oh_mygod.js','ops_hi.js','web_font.js' ]
icon_font.js替换下划线成功！
oh_mygod.js替换下划线成功！
npm_pkg.js替换下划线成功！
ops_hi.js替换下划线成功！
web_font.js替换下划线成功！
``` 

这时候你再看file目录下面，所有"_"连接的文件名称，全部变为"-"链接。

## 结语

虽然这个🌰比较简单，但是主要是为了提供了一个思路给大家，如何结合自己的工作中的情况来解决重复劳动的问题。其实这个🌰就是利用Nodejs强大的处理文件的能力，提高自己的生产效率，类似这样的问题其实会有不少，怎么去在工作中发掘并解决还是需要经常去思索的.

完...


