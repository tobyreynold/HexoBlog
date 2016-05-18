---
title: nodejs缓冲模块Buffer基础
date: 2016-05-18 15:49:00
tags: Buffer
---

## nodejs Buffer模块简介

在nodejs中，Buffer模块是和Node内核一起发布的核心模块，Buffer模块可以让nodejs处理二进制数据，一个Buffer类似一个整数数组，但是它不存在V8堆内存内，大家知道一般32位系统V8引擎大概。会用掉0.7G的内存，64位的系统大概会用掉大约1.4G的内存，这都是给V8用的内存空间，是堆内内存，但是Buffer是堆外的，不会和V8公用一处内存的，这点必须要要清楚。

## Buffer支持的几种编码格式

* 'ascii'
* 'utf-8'
* 'ucs2'
* 'base64'
* 'binary'

## Buffer的基本使用

Buffer主要分为三个部分，创建Buffer，读取Buffer，写入Buffer，🌰如下：
### 创建Buffer
``` bash
var a = new Buffer(10); /* 新建Buffer */
console.log(a);
``` bash

