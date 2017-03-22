---
title: Nginx反向代理
date: 2016-05-14 09:47:05
tags: Nginx
---

## 引言

![Nginx](http://nginx.org/nginx.png)
网上讲Nginx的文章也很多，各种入门的深入的也都有，这里不做太多过深的研究，只是罗列一下我在配置过程中遇到的问题以及如何解决的。

## 反向代理

代理估计大家也都知道是什么意思，按照字面意思反向代理的意思也不难理解，Nginx作为一个高性能的HTTP和反向代理服务器，现在已经被各大互联网公司采用，也可以作负载均衡，它的性能各方面比Apache优势要明显很多，所以这也是它现在这么受欢迎的原因。

## 端口的转发

如果你的Server不安装Nginx，那么备案之后，以及默认配置，外网访问的都是80端口，当然你也可以把服务启动起来，指定80端口就好了，外面用户敲域名回车也能访问你的网站。但是如果一台Server想通过多个不同端口启动多个服务，配置很多的子域名打到对应的端口的服务的时候，Nginx就必须搞起来了。

## 安装Nginx

一般新买的服务器都会有Yum环境，如果没有，自行安装下，Yum命令是在Fedora和RedHat以及SUSE中基于rpm的软件包管理器，主要是方便系统管理人员更精细化更快速的管理和安装各种软件包，Yum环境有了之后，就可以按照这个链接[Nginx安装教程](https://mos.meituan.com/library/18/how-to-install-lnmp-on-centos7/)给的教程安装了。

## 增加子域名

域名在哪里买的，就去那个服务商网站登录，进入域名解析设置里面，多设置一个A记录类型，主机记录是你起的子域名名称(举个 🌰 ：你设成abc，那么你就有了一个abc.domain.com的子域名)，然后记录值还是你的Server的公网ip地址，TTL选择最短的时间，然后保存，一般很快就能生效了，这样你就给你的域名开了一个新的子域名。

## Nginx.conf如何配置

举个 🌰 ，就拿我自己的站点来说，除了默认的codefilled.com和www以外，我还有一个[m.codefilled.com](http://m.codefilled.com/)的子域，现在我想启动两个服务，端口号一个是4000和3000，主站服务端口号4000（但是用户默认访问的是80），m站的端口指到3000端口上面启动的服务，这样的需求，Nginx应该如何配置，可以看一下下面的配置代码:
``` bash
include /etc/nginx/conf.d/*.conf;
server_names_hash_bucket_size 64;
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  codefilled.com   www.codefilled.com; /*配置多个，也可以拿正则来匹配，例如：www.* */
    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
       proxy_pass  http://127.0.0.1:4000;
    }

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}
```
*注：上面的配置是监听的80端口，3w和没有3w的80统一都转发到4000端口的服务，server_names_hash_bucket_size 64 增加这行的原因是添加多个server_name之后，保存Nginx配置会提示server_names_hash_bucket_size不足，把size变大之后就能解决，如果64不够，你再用128这样就可以了。

``` bash
server {
	listen 80;
	server_name m.codefilled.com;

	include /etc/nginx/default.d/*.conf;

	location / {
	     proxy_pass http://127.0.0.1:3000;
  }
}
```
*注：上面这是配置是把m站的80端口代理到3000。

修改完etc/nginx/nginx.conf之后，保存，执行下面的命令:
``` bash
$ nginx -s reload
```

就能生效了，然后启动各自服务的守护进程，就能正常访问了。

## Nginx配置整站gzip压缩
gzip是一种改进web应用程序性能的一种压缩技术，大流量的WEB站点常常使用GZIP压缩技术来让用户感受更快的速度。这一般是指服务器中安装的一个功能，当有人来访问这个服务器中的网站时，服务器中的这个功能就将网页内容压缩后传输到来访的电脑浏览器中显示出来。一般对纯文本内容可压缩到原大小的40%左右。这样传输就快了，效果就是你点击网址后会很快的显示出来。配置的代码如下：
``` bash
 # open gzip
 gzip                on;
 gzip_min_length     1k;
 gzip_buffers        4 16k;
 # gzip_http_version 1.0;
 gzip_comp_level     2;
 gzip_types   text/plain application/x-javascript application/javascript  text/css application/xml text/javascript image/jpeg image/jpg image/png image/gif
 gzip_vary    on;
 gzip_disable "MSIE [1-6]\.";
```
大致意思就是开启gzip，大于1K的才进行压缩，压缩级别1-10，压缩类型js、主文档、jpg、png、css、gif，IE1~6对gzip支持不好，关闭。

## 查看Nginx哪些端口被占用

执行 $ss -tnl
``` bash
$ss -tnl
State      Recv-Q Send-Q         Local Address:Port        Peer Address:Port
LISTEN     0      128            *:80                      *:*
LISTEN     0      128            *:22                      *:*
LISTEN     0      100            127.0.0.1:25              *:*
LISTEN     0      128            *:4000                    *:*
LISTEN     0      128            :::8080                   :::*
LISTEN     0      128            :::80                     :::*
LISTEN     0      128            :::22                     :::*
LISTEN     0      128            :::3000                   :::*

```
就能看到结果列表了

完...






