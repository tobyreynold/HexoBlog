---
title: MkDocs快速搭建
date: 2016-06-01 21:05:57
tags: MkDocs
---

## MkDocs是什么?

MKDocs就是一个能快速，简单，优雅的生成静态文档站点的项目。文档用Markdown书写，目录用YAML配置，弄好之后你可以把它放到Github的Pages 上，或者自己服务器上就可以了。并且也有好几套UI供你选择[MkDocs UI](https://github.com/mkdocs/mkdocs/wiki/MkDocs-Themes)，特别适合那些做开源项目的童鞋，生成说明文档使用。总的来说，就是省时省力，效果还不错。

## 安装

前提你电脑上已经有了Python，以及Python的包管理器Pip，一般都会有Python环境，Pip不一定有，Pip的安装移步这里[Pip Install](https://pip.pypa.io/en/stable/installing/)，进去之后你会发现其实Pip的这个说明文档，也是用这个鬼（MkDocs）做的，其实还是很多人都在用的吧。。😄
这里提一嘴，你的新建一个get-pip.py文件，从上面的安装教程里面下载一段Python代码拷进去，然后执行下面命令，Pip才能安装成功

``` bash
$sudo python get-pip.py /* 最好加上sudo，否则最后有可能某些文件权限不够安装失败*/
$pip --version
pip 1.5.2
``` 

Pip安装成功之后，接着安装Mkdocs，执行下面的命令

``` bash
$sudo pip install mkdocs /* 最好加上sudo，否则最后有可能某些文件权限不够安装失败*/
$mkdocs --version
mkdocs, version 0.15.2
``` 
## 开始

准备工作做完了，就可以开始了，执行下面命令：

``` bash
$mkdocs new my-project
$cd my-project
$ mkdocs serve
Running at: http://127.0.0.1:8000/
``` 

这时候就可以在本地8000端口看到了，进入my-project，会有一个doc目录和一个mkdocs.yml文件，.yml就是配置文件，配置如下：

``` bash
site_name: Mkdocs
pages:
- Home: index.md
- About: about.md /*在doc目录下新建.md文件就好了，冒号前面的key会自动生成目录名称*/
theme: readthedocs /*选择了readthedocs主题*/
``` 

上面这个配置基本就是[Pip Install](https://pip.pypa.io/en/stable/installing/)和这个效果差不多的，我这里就不给demo了，您也可以按照上面的教程实现以下，其实就是这个链接的效果。
也可以进行以下其他的操作，例如：

``` bash
$mkdocs build /*生成网站的site目录，里面有一些静态资源*/
$mkdocs build --clean
$mkdocs --help
``` 

## 结语

总之，这还是一个比较方便的工具，还是那句话，特别适合开源项目快速撰写文档，或者API接口文档等，如果没记错的话，点评的内部一些文档，也是用的类似工具快速生成的。


完...










