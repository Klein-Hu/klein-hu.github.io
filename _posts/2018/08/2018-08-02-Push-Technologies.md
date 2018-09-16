---
layout: post
title: Push Technologies
categories: [Reading]
tags: [Career, Architecture]
fullview: false
comments: true
---

这周跟朋友讨论起了server push，之前零星知道一些push technologies，于是想看看是不是有更多的新老技术。[Wikipedia](https://en.wikipedia.org/wiki/Push_technology)永远是个宝！简单整理个列表备忘用吧：

* Webpush：HTTP 2.0特有，主流浏览器都支持
* HTTP server push：基于WebSocket的全双工TCP connection
* Pushlet：基于HTTP persistent connection（也叫[HTTP keep-alive](https://en.wikipedia.org/wiki/HTTP_persistent_connection)或者HTTP connection reuse），client依赖server的timeout setting，通常5-15 seconds。而如果client忘记关闭连接，server不能释放资源
* Long Polling：假装的Push，比pushlet轻量，但需要server hold connection和client的重连逻辑
* Flash XMLSocket relays：读写分离，建立一个Unidirectional network，放一个relay，relay push id，client用id来fetch content。对读写不对称的情况尤其高效。scale的瓶颈在于http stack能容纳多少个concurrent连接。
* Reliable Group Data Delivery (RGDD)：主要用来进行数据备份，比如HDFS的replication
* Push Notification：Apple, Google and Microsoft support it.
  * Local: scheduled event / timer event
  * Remote: app注册，server用http发notification

当然，这里没有包括我心爱的[WAP Push](https://en.wikipedia.org/wiki/Wireless_Application_Protocol#WAP_Push)，一个只对手机移动网络有效的Push。知道这个的人估计会越来越少啦。