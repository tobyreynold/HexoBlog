---
title: npm必须掌握的基础概念
date: 2016-05-16 17:07:15
tags: npm
---

## 什么是npm？

npm其实就是一个包管理器，是方便JavaScript开发者分享和复用代码而诞生的，现在npm上面已经有丰富的module供开发者使用了，对于开发者这是一种新的共享和复用代码的方式，而且还能更方便的维护各种代码版本之间的关系。
![npm](https://docs.npmjs.com/images/npm.svg)

## 安装npm

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

## npm 发布自己的module
基本的步骤如下：
 ``` bash
 $npm login
 $npm init 
 $npm config set scope username
 $vim /{project}/{index.js}
 $npm publish --access=public /*--access=public参数是选择性的，如果不添加则该package只能指定用户看到*/
 ``` 
自己的module如何进行更新，动态版本号是npm的一个比较方便的约定，分为三种，patch，minor，major，举个例子：
* Patch releases: 1.0 or 1.0.x or ~1.0.4，顾名思义patch就是补丁的意思，更新的是第三位数字
* Minor releases: 1 or 1.x or ^1.0.4，Minor意思是较少的，更新的是第二位数字
* Major releases: * or x，Major意思是主要的意思，更新的是第一位数字
那么问题来了，当你的一个npm package进行了一些补丁修复或者是主要的更新之后，怎么样生成新版本号，执行下面的命令就可以了：
``` bash
$npm version patch
$npm version minor
$npm version major
``` 
与此同时package.json里面的版本号就会动态更新了。
node.js 的模块管理中，最常用到的几种动态版如下：
\*: 任意版本
1.1.0: 指定版本
~1.1.0: >=1.1.0 && < 1.2.0
^1.1.0: >=1.1.0 && < 2.0.0
其中 ~ 和 ^ 主要有两种前缀，简单的来说：
~ 前缀表示，安装大于指定的这个版本，并且匹配到 x.y.z 中 z 最新的版本。
^ 前缀在 ^0.y.z 时的表现和 ~0.y.z 是一样的，然而 ^1.y.z 的时候，就会 匹配到 y 和 z 都是最新的版本。
特殊的是，当版本号为 ^0.0.z 或者 ~0.0.z 的时候，考虑到 0.0.z 是一个不稳定版本， 所以它们都相当于 =0.0.z。
~其实就是从尾部开始限制序号（0除外），^其实就是从头部开始限制序号（0除外）。
还有一种情况是2.1.\*是指major和minor固定，patch是任意的.
如果实在想不明白请移步这里[semver](https://www.npmjs.com/package/semver),这个npm package可以帮你，下面也有详细的注释讲版本号到底应该怎么算的。

## npm 增加 tags

npm publish 默认是发布的最新版本，如果参数增加--tag {name}，则该版本就有了{name}的一个代号，这样如果npm install的时候就可以安装某个代号版本的npm package。
``` bash
$npm publish --tag beta
$npm install somepkg@beta
``` 

上面这些知识点基本可以算是npm 入门必备知识了，后续会陆续更新npm进阶知识。

完...












