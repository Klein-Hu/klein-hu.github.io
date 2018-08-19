---
layout: post
title: PInterest Architecture (Apr. 2013)
categories: [Reading]
tags: [Career]
fullview: false
comments: true
---

文字：[PInterest Architecture](http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)
讲座：[PInterest Architecture](https://www.infoq.com/presentations/Pinterest)

本文亮点有两个：

1. 平台的演化过程
2. 数据库从没有sharding，到纠结要不要sharding，到怎么样sharding，到怎么做migration

## PInterest 平台演进

这篇文章最大的亮点在于把一个网站从无到有的架构变化一点一点的描述出来了。我整理成一个结构图如下：

![PInterest平台演进]({{ site.url }}/assets/images/PInterest.svg)

## 技术选型的参考

* Why Amazon EC2/S3
* Why MySQL
* Why memcache
* Why Redis
* Why Solr

## 经验教训

* When you push something to the limit all these technologies fail in their own special way.
* 使用成熟的技术，不但使得产品成熟度高，而且能够得到社区的支持。另外还可以雇到直接就会这些东西的工程师而不是来了以后再学。
* 选择简单的技术，比如memcache或者redis，更容易得到稳定的运行，出了问题也容易排查。
* 丢了一部分data还不如data全丢了，因为确定哪部分data丢了是一件非常困难的事情。

## Cluster vs Shard

* Cluster: 自动化程度高，但复杂度同样很高，自动distributed，数据移动不是问题->Rebalance容易，可以做join
    * Pros: 
        * Auto-scale
        * Easy to setup.
        * Data redundence容易
        * High availability
        * Load balancing
        * No single point of failure
    * Cons:
        * 新技术
        * 底层技术复杂度高事情
        * 社区支持少，因为被产品分成了小社区，互相难分享经验
        * 升级困难，容易出现版本混乱
        * Cluster Management Algorithm is a SPOF (Single Point Of Failure)
        * Cluster Management failure types:
            * Data rebalance breaks, when stuck on replication.
            * Data corruption, when there is a bug in writing log
            * Improper balancing not easy to fix
            * Data authority failure: cluster secondary may claim it is primary and causing data lost. 
* Shard: 基本上纯手工，但复杂度低且效率高。另外数据不能移动。Node之间互相不知道对方存在，master node控制一切。此处结构很像[Flickr]( http://highscalability.com/blog/2007/11/13/flickr-architecture.html)
    * Pros:
        * 切分的数据库，增加新的capability也很容易
        * 空间上分布化数据
        * 高可用
        * Load balancing
        * 决定Sharding的算法是整个系统里的SPOF，但这个算法非常简单直接，行与不行半天就能得出结论；相对复杂的Cluster management算法，这个即使出问题也很容易早发现早处理。
        * ID生成简单
    * Cons:
        * 对大部分的join无效
        * 对一个shard的写入失败的时候可能对另一个成功，所以trasaction capability不能保证
        * 因为不能join，所以很多数据处理和constrain要放在application里面
        * Schema的改动很麻烦
        * Reporting要在每个shard上跑query，然后再汇总
        * 客户端程序复杂度高，要能够处理以上所有的问题

## Transition to Shard

* Feature Freeze
* 动尽量少的query，动尽量少的data
* 之前的很多join没用了，可以删了
* 每一个query都需要加上cache
* 步骤：
    * 1 DB + Foreign Keys + Joins
    * 1 DB + Denormalized + Cache
    * 1 DB + Read Slaves + Cache
* Several functionally sharded DBs + Read slaves + Cache
* ID sharded DBs + Backup slaves + cache
* 之前的slave会导致延迟：数据在slave上，但master的还没transit到shard上。此时需要cache。
* Background script来保证duplicate数据
* User table不做shard，用一个单独的数据库来保存。主要用来查询用户名conflict。

## How to define ID structure

* 64-bit
    * Shard ID: 16 bits, build location into ID, save the 3ms look up in AWS+MySQL
    * Type: 10 bits
    * Local ID: rest of bits, MySQL auto increment
* 新用户随机分布
* 跟一个用户有关的信息集中到同一个Shard上
* 不要一次用掉所有的Shard：可以支持到65536个，但开始的时候只用4096个

## 其他

* ID主要是用来帮助Sharding算法的，所以怎么设计ID的结构很有意义
* 所有的数据，要么是Object，要么是Object之间的Mapping
* 存数据的时候用key-value pair，value是个blob。这样以来更改field不会影响整体schema。每一个blob都含有version信息，如果需要升级version，可以在下一次读到的时候再改，不用一次migrate所有数据。
* 当有了新旧两套系统的时候，必须得两个系统都写入（没说如果其中一个写失败了咋办）
* 新旧系统需要灰度切换或者蓝绿切换
* Pyres，Redis-based message queue，支持priority和retry，可以替换Celery + RabbitMQ

## Lessons Learned

* When you push something to the limit all these technologies fail in their own special way.
* Architecture is doing the right thing when growth can be handled by adding more of the same stuff.
* The least data you move across your nodes the more stable your architecture.
* Don’t freak out about losing a little data.