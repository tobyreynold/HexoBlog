---
title: npm必须掌握的基础概念
date: 2016-05-16 17:07:15
tags: npm
---

# 什么是npm？

npm其实就是一个包管理器，是方便JavaScript开发者分享和复用代码而诞生的，现在npm上面已经有丰富的module供开发者使用了，对于开发者这是一种新的共享和复用代码的方式，而且还能更方便的维护各种代码版本之间的关系。
![npm](https://docs.npmjs.com/images/npm.svg)

# 安装npm

一般只要安装nodejs之后，npm也会自动安装成功，如果npm版本太低，可以执行下面的命令进行npm版本的更新：
``` bash
$sudo npm install npm -g
``` 
npm上发布的包，基本都可以通过下面这个链接看到每个包的具体信息情况：
https://registry.npmjs.org/XXX（XXX是包的名字）
例如[memwatch](https://registry.npmjs.org/memwatch)的各种历史版本信息(一个nodejs的内存监控工具)。
npm install是一个经常会用到的命令，--save和--registry参数也会经常使用
``` bash
$npm install XXX --save --registry=XXX
``` 
--save的意思：安装完成之后，会自动写入到你的packjson.json的dependencies里面。
--registry的意思：你这个包可以从哪个源来下载，不加这个参数的时候默认是从[npm](https://www.npmjs.com/)上面来下载，但是由于GFW的原因，有时候可能会下载失败，这时候我们也可以添加国内的镜像，例如：[淘宝的](https://npm.taobao.org/)，[美团的](http://npm.sankuai.com/)，淘宝的的镜像每隔10分钟会与官方服务器同步一次，用淘宝的源一般不会安装失败。
卸载某一个包就是把install换成uninstall就好了

未完待续...


