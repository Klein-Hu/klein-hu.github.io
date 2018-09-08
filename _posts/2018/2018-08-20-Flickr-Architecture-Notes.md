---
layout: post
title: Flickr Architecture (Sep. 2007)
categories: [Reading]
tags: [Career, Architecture]
fullview: false
comments: true
---

文字：[Flickr Architecture](http://highscalability.com/flickr-architecture)

相关讲座:

* [Scalable Web Architectures: Common Patterns and Approaches]( https://www.slideshare.net/adunne/scalable-web-architectures-common-patterns-and-approaches-155959/138-Flickr_ArchitectureWeb_20_Expo_Berlin)
* [Scalable Web Architectures: Common Patterns and Approaches - Web 2.0 Expo NYC](https://www.slideshare.net/iamcal/scalable-web-architectures-common-patterns-and-approaches-web-20-expo-nyc-presentation?qid=9a1d7870-1bac-4fc2-b3cd-ff73756c0adc&v=&b=&from_search=1)
* [Architecture Patterns - Open Discussion (Facebook)](https://www.slideshare.net/blue9frog1/architecture-patterns-open-discussion?qid=9a1d7870-1bac-4fc2-b3cd-ff73756c0adc&v=&b=&from_search=8)

## 环境

* LAMP
* DB:
  * MySQL
  * Sharding
* Reverse Proxy:
  * Squid: HTML + image
* Templating
  * Smarty
* Script:
  * Perl
* Image processing:
  * ImageMagick
* XML and email parsing:
  * PEAR
* Node service:
  * Java
* Deployment
  * SystemImager
  * Subcon stores essential system configuration files in a subversion repository for easy deployment to machines in a cluster.
* System monitoring
  * Ganglia
* Distributing and updating collections of files across a network.
  * Cvsup

## Architecture

![Flickr Architecture]({{ site.url }}/assets/images/Flickr_Architecture.png)

## Learning Points

* Architecture
  * “Share Nothing”
  * Stateless
  * Scale replication，只对read有帮助
* Static Content
  * Dedicate server
* Content
  * 特殊处理Unicode 
  * 除了photo，其他的都在DB里
  * "Search Farm"：只针对DB中需要search的东西做replication
* DB
  * Sharding
    * My data gets stored on my shard, but the record of performing action on your comment, is on your shard. When making a comment on someone else's’ blog
  * Each server in shard is 50% loaded. So 1 server in the shard can take the full load if a server of that shard is down or in maintenance mode.
  * Each Shard holds 400K+ users data.
* Backup
  * 定期hot backup
  * 每天夜里snapshot所有DB
  * 同时写入或者删除
* 处理tag
  * 普通normalized RDBMs schema不适用，得做denormalization + heavy cache来解决
  * 如果一些数据可以提前处理就offline做
  * 方向：real-time BCP
* Lesson Learned
  * 从app角度考虑问题，而不是web application
  * Stateless更容易简化流程
  * 尽早引入capacity plan
  * 尽早引入telemetry
  * Caching and RAM is the answer to everything.
  * Abstraction + Layers
  * 最好能每30分钟一个release
  * NO PREMATURE OPT
  * Benchmark: artificial tests give you artificial result. Telemtry from real production is the king
  * 提前找到每个部分的上下限，这样来考虑极端情况
  * 根据历史数据，找到用户使用的pattern，于是就可以plan for peak了