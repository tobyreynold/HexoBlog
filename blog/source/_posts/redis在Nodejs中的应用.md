---
title: Redis在Nodejs中的应用
date: 2016-07-10 21:14:33
tags: Redis
---

## 什么是Redis？

Redis是一个开源的拿C语言编写的可持久化的日志型、Key-value的数据库，提供多种语言的API，Nodejs可以以很容易的使用。类似的还有Memcached，Redis和Memcached有一样的地方也有不一样的地方，一样的地方比如：他们都是将数据存到内存中，提高读写速度，避免直接对数据组直接进行读写操作，不一样的地方有很多：Memcached是多线程非阻塞I/O模型，Redis是单线程的I/O复用模型，对于单线程可以将速度优势发挥到最大，实现了epoll，Nodejs的V8核心其实也是epoll，这点上来说两者有着共同的东西，具体Redis和Memcached之间的区别可以移步，[Redis与Memcached的区别](http://blog.csdn.net/tonysz126/article/details/8280696/)。因为是纯内存操作，Redis的性能非常出色，每秒可以处理超过10万次读写操作，貌似是性能最快的Key-Value DB.![NodeRedis](https://cloud.githubusercontent.com/assets/1152927/8677879/35748710-2a17-11e5-8cc6-7aeb72caaa52.png)


## 安装Redis

#### 下载安装

可以先去中文官网下载压缩包[Redis官网](http://www.redis.net.cn/)，也可以用命令来下载安装，命令如下：
``` bash
$ wget http://download.redis.io/releases/redis-3.0.6.tar.gz 
$ tar xzf redis-3.0.5.tar.gz
$ mv redis-3.0.5 redis
$ cd redis
$ make
$ make test
$ make install
```

#### 配置Redis.conf

redis解压目录里有一个配置文件redis.conf ，编辑此配置文件，找到dir ./这一行，Redis会将数据写入文件中，而这里配置的就是将指定数据保存到对应路径，我本机的指定目录是：
``` bash
$ dir /Users/{用户名}/redis_data/
```
编辑过后，将配置文件移动到 /usr/local/etc 目录下：
``` bash
$ sudo mv redis.conf /usr/local/etc
```

#### 启动Redis

命令行输入：
``` bash
$ /usr/local/bin/redis-server /usr/local/etc/redis.conf
```
启动成功画面：
``` bash
13417:M 10 Jul 16:52:00.552 * Increased maximum number of open files to 10032 (it was originally set to 256).
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 3.0.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 13417
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'
13417:M 10 Jul 16:52:00.553 # Server started, Redis version 3.0.6
13417:M 10 Jul 16:52:00.554 * The server is now ready to accept connections on port 6379
```
如果你有写入操作，控制台就会出现信息，类似于：
``` bash
13417:M 10 Jul 16:52:00.553 # Server started, Redis version 3.0.6
13417:M 10 Jul 16:52:00.554 * The server is now ready to accept connections on port 6379
13417:M 10 Jul 17:00:33.754 * 10 changes in 300 seconds. Saving...
13417:M 10 Jul 17:00:33.754 * Background saving started by pid 13458
13458:C 10 Jul 17:00:33.756 * DB saved on disk
13417:M 10 Jul 17:00:33.856 * Background saving terminated with success
```

## Nodejs安装Node_Redis

#### 安装
``` bash
$ npm install redis --save
```

#### 使用教程

Nodejs for Redis 专门有一个官方文档，查询使用方法可以移步到[Nodejs版Redis使用文档](http://redis.js.org/).

#### Demo

下面是一个简单的demo，大概意思就是新建一个Redis的client，默认的这个Nodejs的client会去连接本地127.0.0.1:6379是否有Redis服务，如果连接成功，则可以往上面配置的指定路径中写入dump.rdb文件，数据文件一般存放在内存中，只要不断开连接，随时可以进行读写，最下面的那个forEach大概意思就是把Redis中Key是"hash key"的字段全部遍历出来。
``` bash
var redis = require("redis"),option = {},client = redis.createClient(option);

client.on("error", function (err) {
    console.log("Error " + err);
});
client.set("string key", "string val", redis.print);
client.hset("hash key", "hashtest 1", "some value", redis.print);
client.hset(["hash key", "hashtest 2", "some other value"], redis.print);
client.hkeys("hash key", function (err, replies) {
    console.log(replies.length + " replies:");
    replies.forEach(function (reply, i) {
        console.log("    " + i + ": " + reply);
    });
    client.quit();
});
``` bash
Nodejs执行之后，控制台会打出这样的结果：
``` bash
Reply: OK
Reply: 0
Reply: 0
2 replies:
    0: hashtest 1
    1: hashtest 2
```

## 未完待续

后续可以关注[Codefilled Demo](http://demo.codefilled.com/)，会在这里完成一个Redis的Demo，大概思路是一个可视化的界面，实现Redis的各种操作。

## 结语

既然Nodejs其实已经算是后端语言了，即便被后端开发多么看不上，但是Nodejs定位已经如此了，还是尽量的能做更多的事情比较好，否则只做个控制器这么薄薄一层就显得像一个玩具了，如果选用了Nodejs作为技术栈还是希望能把离前端最近的那一层给干掉比较合适，否则各大公司天天喊着要前后端分离的意义就不是特别大了。后续会陆续推出一些后端技术相关的文章，消息队列，Nodejs use kafka，RPC(node_thrift),zookeeper等..

完..














