---
title: Javascript SourceMap syntax changed
date: 2016-05-27 22:51:24
tags: SourceMap
---

## Javascript SourceMap 简介

SourceMap的入门知识请移步阮一峰之前写的[JavaScript sourceMap详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html),这里不做太多的入门介绍，总的一句话，这破玩意就是在压缩后的js代码最后加入"//@ sourceMappingURL=jquery.min.map"一行代码，作用呢说起来也很简单，就是能帮你找到你检查的某句代码在哪个文件，第多少行，举个 🌰 ：点击后面这个链接进入[Sourcemap demo](https://www.thecssninja.com/demo/source_mapping/),里面就是你选中代码里的一段，右键选择get origin location，上面的Output框里面就打印出来你选中的代码在哪个文件的第多少行。

## Syntax changed

SourceMap是谷歌出的，现在也更新到了第三版，以前都是"//@ sourceMappingURL=XXX.min.map"这么加，现在如果还是这么写的话，Chrome控制台就会报一个这样的错，"/*@ sourceMappingURL=" source mapping URL declaration is deprecated, "/*# sourceMappingURL=" declaration should be used instead。这个是最近看美团i版线上控制台出现的，以前也没有，其实就是把之前的@号变成#好就好了，源于[Google Developer的文档](https://developers.google.com/web/updates/2013/06/sourceMappingURL-and-sourceURL-syntax-changed)。

## 扩展

阮一峰的文章里面说最常用的方法是使用Google的Closure编译器，链接进去之后也不知道那是什么鬼，看底下的介绍说拿java执行一个命令就能生成sourcemap文件，前端工程师用什么java，后又在google上搜看到一篇比较靠谱英文的文章，仔细原来峰哥就是把那篇文章翻译过来了，链接在这[Html5rocks SourceMap](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/#toc-howgenerate)，估计他自己也没亲自试验过吧 😓，于是自己还是不清楚怎么来生成，哥觉得自己在研究一下，遂明白，Grunt和Gulp都有uglify的插件，uglify可以选择性的生成sourcemap，请移步[uglify 2](https://github.com/mishoo/UglifyJS2#the-simple-way)，里面有详细的说明怎么样生成sourcemap。

## 总结

总之一句话，直接搬过来翻译一下还是不太好😄，盲目照搬照抄没自己的观点也不行哈，毕竟现在是一个多元化的社会。


完...


