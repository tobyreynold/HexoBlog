---
title: 搭建Codefilled下的demo服务
date: 2016-07-03 21:44:54
tags: Demo
---

## 引言

有时候光写文章贴代码其实是一个挺烦的事情，贴了代码上去，读者不一定愿意看，而且还比较占篇幅，一篇文章可能往下滚半天才能看完，还是需要一个存放Demo的路径比较合适，其实早都想这样弄了，刚好接着捣鼓定位Demo的契机，把[demo.codefilled.com](https://demo.codefilled.com/)搭建起来，这样我的这台服务器上现在有三个Nodejs的服务同时在跑，一个主页的Hexo，一个聊天室[m.codefilled.com](https://m.codefilled.com/)，还有[demo.codefilled.com](https://demo.codefilled.com/).

## 内容

现在只放了两个Demo扔在上面，以后随着边写文章，需要配合Demo一起的展示的，都丢到[demo.codefilled.com](https://demo.codefilled.com/)上面去。

* HTML5 电池🔋的API的Demo[H5🔋电池 API](https://demo.codefilled.com/battery).

* HTML5 GeoLocation定位的Demo，[H5定位](https://demo.codefilled.com/geolocation)，测试接入的是腾讯的定位，参考文档[腾讯地图开放平台](https://lbs.qq.com/tool/component-geolocation.html).

* HTML5 GeoLocation定位携带Map的Demo，[H5带Map展示的定位Demo](https://demo.codefilled.com/geolocation/maps).
 
## 结语

HTML5的那个🔋Demo 纯是为了玩，定位这个是腾讯这边对H5 GeoLocation做了优化改进的定位组件，比较适合移动H5开发，针对经常会在微信、手Q里面使用定位功能的场景，做了专门针对腾讯系列社交app内嵌的优化。

完...