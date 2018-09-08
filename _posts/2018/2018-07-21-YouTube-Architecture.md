---
layout: post
title: YouTube Architecture (Jun. 2007)
categories: [Reading]
tags: [Career, Architecture]
fullview: false
comments: true
---

原文：[YouTube Architecture](http://highscalability.com/youtube-architecture)

视频：[YouTube Architecture](https://www.youtube.com/watch?v=w5WVu624fY8)

## Python in Production

因为项目组有一个service正好是Python写的，无数的人抱怨“慢”，但数据显示overhead并不大。所以对YouTube早期的Python stack非常感兴趣！

* CPU不是瓶颈，至少在Web Server这个层面不是，RPC才是！
* 选择Python，是开发效率和运行效率的折衷
* 如何尽可能的提高Python的效率：

  - 使用[Psyco](https://en.wikipedia.org/wiki/Psyco)作JIT。不过这个项目今天已经死了，但想法是类似的。
  - 一部分代码写成c extension，这个深有体会
  - 能事先生成一些HTML就尽量生成，节省runtime的时间

* 不要用Apache，YouTube当时选择了lighttpd。理由是Apache的线程数多而内存效率低。
* 充分利用CDN，这一点我们也是能用GET的API就不用POST。
* 意料之外的挑战性工作：支持Video Thumbprint

  - 每个video四个thumbprint，访问量巨大，Disk seek都是问题
  - Video thumbprint的个数达到每个文件夹里的文件数量上限

## 数据库
  
* MySQL
* 此处需要注意page cache的设置，如果大于了设置的值就会导致swapping，性能大打折扣
* 优化

  - Query Optimization，这个最常用，针对query具体做
  - 尽量Batch job，但如何知道哪些jobs之间可以batch是个技术活，需要具体问题具体分析
  - memcache，现在估计是Redis了
  - Replication
    
    * 用记录回放来保证最终一致性
    * 把replica db分成general purpose和video watch两个pool来均衡负载
    * RAID 0给Software，RAID 1给Mirrors
  
  - Sharding：保证随时可以移动shard来重新balance

##  简单总结

Python + MySQL的基本架构至今仍然不弱，小网站依然可以用这俩撑起来。