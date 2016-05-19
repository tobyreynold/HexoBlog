---
title: nodejs缓冲模块Buffer基础
date: 2016-05-18 15:49:00
tags: Buffer
---

## nodejs Buffer模块简介

在nodejs中，Buffer模块是和Node内核一起发布的核心模块，Buffer模块可以让nodejs处理二进制数据，一个Buffer类似一个整数数组，但是它不存在V8堆内存内，大家知道一般32位系统V8引擎大概。会用掉0.7G的内存，64位的系统大概会用掉大约1.4G的内存，这都是给V8用的内存空间，是堆内内存，但是Buffer是堆外的，不会和V8公用一处内存的，这点必须要要清楚。

## Buffer支持的几种编码格式

* 'ascii' -  仅用于 7 位 ASCII 字符。这种编码方法非常快，并且会丢弃高位数据。
* 'utf-8' - 多字节编码的 Unicode 字符。许多网页和其他文件格式使用 UTF-8。
* 'ucs2' -  两个字节，以小尾字节序(little-endian)编码的 Unicode 字符。
* 'base64' - Base64 字符串编码。
* 'binary' - 将原始二进制数据转换成字符串的编码方式，仅使用每个字符的前8位。这种编码方法已经过时，应当尽可能地使用Buffer对象。Node 的后续版本将会删除这种编码。

## Buffer的基本使用

Buffer主要分为三个部分，创建Buffer，读取Buffer，写入Buffer， 🌰 如下：
### 创建Buffer

``` bash
var a = new Buffer(10); /* 新建Buffer */
console.log(a);
```

未完待续...

