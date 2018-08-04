---
layout:     post
title:      "一次Redis被攻击的体验"
date:       2018-03-05 12:00:00
author:     "chengyu.yxm"
header-img: "img/in-post/2018/Redis-Attack/bg.jpg"
tags:
    - 网络安全
    - 服务器
    - Redis
---

今天本来高高兴兴地在开发项目，当我打开redis远程管理的时候突然发现，数据库中多了两条奇怪的数据。
![](/img/in-post/2018/Redis-Attack/n1.jpg)
<center>n1</center>
![](/img/in-post/2018/Redis-Attack/n2.jpg)
<center>n2</center>


我当时就发现这个ip我不认识啊，而且这个明显是cron的定时脚本，我心想不会是我开发的时候不小心把一些什么奇奇怪怪的东西写到redis里了吧。但是我的项目缓存没有持久的，这个TTL:-1让我觉得十分奇怪。

抱着好奇，我把这个ip查一了一下 是加拿大的服务器。

![](/img/in-post/2018/Redis-Attack/ip.jpg)
<center>百度结果</center>


我心想我一个阿里云的学生服务器，我也没往墙外走过，这个肯定不是我主动造成。接着我尝试直接访问这个地址，看到的结果令我大吃一惊。

![](/img/in-post/2018/Redis-Attack/db.jpg)
<center>db.php</center>
这是shell脚本啊，我被攻击了。



一直听闻网络黑客神乎其神，但是长在红旗下的我还真没怎么一对一遇到过黑客攻击，这一波我要仔细瞅瞅。问了一下专攻安全的学弟，才知道这个黑客的手段。



大概攻击手段如下

由于弱口令进入redis
本地生成公私钥文件，把公钥文件通过redis >config set 注到我的服务器中
通过本地私钥 ssh –i id_rsa root@xxx.xxx.xx.xx 直接获取服务器
不重视网络安全，活该被攻击，还好这台服务器只是我测试用的，最后解决办法，找到这段rsa删除，redis密码加强。仔细分析代码还被执行了"sr0.db"脚本，有可能是后门脚本。以后只能悠着点了。

![](/img/in-post/2018/Redis-Attack/bqb.jpg)
技术参考：

[Redis 未授权访问配合 SSH key 文件利用分析](http://blog.knownsec.com/2015/11/analysis-of-redis-unauthorized-of-expolit/)