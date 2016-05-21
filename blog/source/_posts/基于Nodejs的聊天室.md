---
title: 基于Nodejs的WebSocket聊天室
date: 2016-05-21 23:20:24
tags: WebSocket
---

## 什么是WebSocket和Socket.io?

![](http://cdn.socket.io/website/imgs/logo.svg)
WebSocket是HTML5的一种通讯协议，实现了浏览器和服务器之间的双向通讯，[Socket.io](http://socket.io/)是一个完全用JavaScript实现的，基于Nodejs和WebSocket的协议做的一个开源框架，Socket.IO除了支持WebSocket通讯协议外，还支持许多种轮询（Polling）机制以及其它实时通信方式，并封装成了通用的接口，并且在服务端实现了这些实时机制的相应代码。Socket.IO实现的Polling通信机制包括Adobe Flash Socket、AJAX长轮询、AJAX multipart streaming、持久Iframe、JSONP轮询等。Socket.IO能够根据浏览器对通讯机制的支持情况自动地选择最佳的方式来实现网络实时应用。


## 准备工作

快速生成一个express框架，安装相应package，命令如下:

``` bash
$express ChatRoom /* ChatRoom工程的名字，如果没有安装express_cli的需要自行安装 */
$cd ChatRoom
$vim package.json /* package.json dependencies里面添加pm2，ejs，socket.io，或者使用npm install对应package */
$npm install --save --registry=http://r.npm.sankuai.com
```

## 第一步

基本的准备工作完成后，需要把express默认的jade替换成ejs，ejs识别.html的作为模板，.html的也比较方便开发人员来搞。在工程的根目录下:

``` bash
$vim app.js
/* 删掉express默认配置jade的两行代码,注册ejs模板为html页,就是原来以.ejs为后缀的模板页，现在的后缀名可以是.html了 */
app.engine('.html', require('ejs').__express);
/* 设置视图模板的默认后缀名为.html,避免了每次res.Render("xx.html")的尴尬 */
app.set('view engine', 'html');
/* 设置模板文件文件夹,__dirname为全局变量,表示网站根目录 */
app.set('views', __dirname + '/views'); 
$:wq  /*保存*/
```

## 第二步

研究express框架，可以知道package.json里面的scripts的start是"node ./bin/www"，其实就是执行的www这个js，修改成"pm2 start ./bin/www"，这样npm start启动就可以用pm2进行守护进程了。

``` bash
$pm2 list
┌──────────┬────┬──────┬───────┬─────────┬─────────┬────────┬─────────────┬──────────┐
│ App name │ id │ mode │ pid   │ status  │ restart │ uptime │ memory      │ watching │
├──────────┼────┼──────┼───────┼─────────┼─────────┼────────┼─────────────┼──────────┤
│ www      │ 2  │ fork │ 81061 │ online  │ 47      │ 16m    │ 38.145 MB   │ disabled │
│ www      │ 3  │ fork │ 0     │ errored │ 15      │ 0      │ 0 B         │ disabled │
└──────────┴────┴──────┴───────┴─────────┴─────────┴────────┴─────────────┴──────────┘
$pm2 stop all/[id|name]  /*可以直接停止对应的进程*/
```

有点跑题了，PM2这里不多讲了，可以直接去[PM2](http://pm2.keymetrics.io/docs/usage/pm2-api/#programmatic-api)的官网自己研究，PM2也提供了一些可编程的API，可以直接写Nodejs来实习PM2的一些指令操作。

## 第三步 

修改www这个js文件，代码如下：

``` bash
$vim bin/www
/**
 * Create HTTP server.
 */
var server = http.createServer(app); /* 在这句代码下面，加入下面的几行代码*/

var io = require('socket.io')(server);
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    io.emit('chat message', msg);
  });
});
$:wq /*保存*/
```

## 第四步 

进入views这个目录，新建一个index.html，加入下面的代码：

``` bash
$vim index.html
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }body { font: 13px Helvetica, Arial; }form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }#messages { list-style-type: none; margin: 0; padding: 0; }#messages li { padding: 5px 10px; }#messages li:nth-child(odd) { background: #eee; }
    </style>
  </head>
  <body>
    <h2>
      <%=title%>
    </h2>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
	<script src="/socket.io/socket.io.js"></script>
	<script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
	<script>
	  var socket = io();
		  $('form').submit(function(){
		    socket.emit('chat message', $('#m').val());
		    $('#m').val('');
		    return false;
		  });
		  socket.on('chat message', function(msg){
		    $('#messages').append($('<li>').text(msg));
		  });
	</script>
  </body>
</html>
$:wq /*保存*/
```

## 第五步

进入routes目录，编辑index.js，代码如下：

``` bash
$vim index.js

var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index',{title:'技术讨论区'});
});

module.exports = router;

$:wq /*保存*/
```
保存完毕之后，这时候代码基本就算弄完了，在工程目录里面 npm start 之后就能在浏览器中看到效果了，本网站已经开放了简易聊天室了，可以直接移步这里看demo，[WebSocket聊天室](http://m.codefilled.com/)。

## 遇到的坑以及解决方法

#### nodejs原生http模块的困惑

socket.io里面教程里面有一段这样的代码
``` bash
var http = require('http').Server(app);
http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

不难理解就是3000端口启动一个http服务，但是通过express cli 生成的./bin/www里面的代码也存放这启动http服务的代码，代码是这样的：

``` bash
var http = require('http');
var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);
var server = http.createServer(app);
server.listen(port);
```

这时候俺就震惊了，同是Require的http，为毛启动一个服务会有两种createServer()和Server()两种？你去查nodejs[官方文档](https://nodejs.org/api/http.html)，发现也只有第一种，那么第二种是什么鬼？

stackoverflow上给出了[答案](http://stackoverflow.com/questions/26921117/http-createserverapp-v-http-serverapp)，其实这时候就得翻nodejs的源码了，源码在这里[nodejs http模块](https://github.com/nodejs/node-v0.x-archive/blob/523929c9272a53c9429616564a45f2af59670e47/lib/http.js#L1903-L1905)，仔细看下第1903行到1905行，就能明白在实现过程中遇到的困惑了。不过从上面的url上可以看到github上看代码的一个小技巧，url后面hash值接L50-L60的意思就是浏览器可以直接定位第50行到60行。

完...







