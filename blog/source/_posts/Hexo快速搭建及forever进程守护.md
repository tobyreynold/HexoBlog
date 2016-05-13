---
title: Hexo快速搭建及forever进程守护
date: 2016-05-12 23:13:13
tags: Forever
---
## Hexo简介
Hexo是一个采用nodejs的静态博客，市面上的类似的博客也有很多，比较有名的Jekyll，Octopress等，到最早的wordPress，我无聊最后选Hexo纯是因为它用的是nodejs而已，其实Jekyll也很好用啊，用Jekyll+Pages一起就能随便搞一个博客，而且还不用自己买服务器，托管到github上还无需备案，无非就是国内访问github有时候有些慢你懂得，闲的无聊也弄了个[tobyreynold](https://tobyreynold.github.io/)，搞起来其实上手比hexo还快，你还可以自己的github账号下面直接建个XXX.github.io的git仓库然后直接一关联就可以了，你也可以买个域名直接指过去，不用他白给你的XXX.github.io也行，随意..
  

## Hexo搭建

进入 [Hexo](https://hexo.io/)官网，按照教程npm install 各种之后，生成Hexo工程，Hexo sever就启动起来了，
同样你在服务器端也可以直接这样搞，或者本地搞定了直接rsync到服务器上去也行，又或者直接github上建一个git仓库，本地搞定了推到github，服务器端pull github上的代码就可以了。


## Hexo进程守护

Node进程守护有很多工具，[Forever](https://www.npmjs.com/package/forever)，[PM2](http://pm2.keymetrics.io/)，[PM2.5](https://www.npmjs.com/package/pm25) blah blah.
解决的问题也是很容易讲清楚，比如：ssh登陆服务器，启动node服务，然后ssh断开连接，服务中断，网站无法访问。
这里讲一下用forever 解决 Hexo 进程守护的问题：
### 安装forever：
``` bash
$ [sudo] npm install forever -g
$ cd /path/to/your/project
$ [sudo] npm install forever-monitor
```

### Hexo下新建一个app.js,写入下面代码：
``` bash
var spawn = require('child_process').spawn;
free = spawn('hexo', ['server', '-p 4000']);/* 其实就是等于执行hexo server -p 4000*/

free.stdout.on('data', function (data) {
console.log('standard output:\n' + data);
});

free.stderr.on('data', function (data) { 
console.log('standard error output:\n' + data);
});

free.on('exit', function (code, signal) {
console.log('child process eixt ,exit:' + code);
});
```

其实思路也很简单，大致意思就是node启动一个子进程，用forever 守护 hexo sever -p 4000这条命令（4000代表端口），关于node的child_process的相关知识，请自行baidu、google,或者去查nodejs的文档。

### 执行forever命令：
``` bash
$ forever --minUptime 10000 --spinSleepTime 26000 start app.js
```

至于后面这几个minUptime、spinSleepTime可填可不填，不填默认也会有，参数的意思可以直接去[forever](https://www.npmjs.com/package/forever)上查询。

### 停止服务

这里值得注意的是你拿forever启动的服务，通过forever stopall是根本停不掉的，因为其实你执行的是hexo sever，可以通过下面的办法：
``` bash
$ lsof -i:[port]
$ ps aux|grep hexo
$ kill pid(进程的id)
```

完...





