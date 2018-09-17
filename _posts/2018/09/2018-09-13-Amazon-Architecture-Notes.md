---
layout: post
title: Amazon Architecture (Sep. 2007)
categories: [Reading]
tags: [Career, Architecture]
fullview: false
comments: true
---

文字：[Amazon Architecture](http://highscalability.com/amazon-architecture)

早期的Amazon结构和一些感触。技术的发展和解决问题的过程值得学习。

## Platform

* Linux
* Oracle
* C++ + Java + Perl
* Java based stack:
  * [Mason](https://cs.gmu.edu/~eclab/projects/mason/)
  * [JBoss](http://www.jboss.org)
  * Servlets

## Architecture

* 如果系统增加了resource就能增加Performance， 系统是scalable的。
* 系统结构最大的改动：two-tier monolith --> fully-distributed decentralized service platform。这个应该是Amazon能有今天的核心原因。
* 把功能封装在service里的好处：
  * Decouple各个模块
  * 各个模块可以用不同的语言和技术来实现
* 系统体量达到一定程度，第三方软件就扛不住了，得自己上。
* 慎重使用middleware，容易被拴在这个framework上
* SOAP : REST = 30 : 70。工程师是不在乎wire上的内容的，哪个都行。
* 当时用WSDL来描述web service，像现在的swagger
* Deployment就是生成一个VM，找个环境去跑。早起的Docker + Kubernetes解决方案。
* **State management is the core problem for large scale systems**
  * 不是所有的操作都是有状态的
  * 如果你能记录所有的行为，是不是有状态也就不重要了
* Eric Brewer's CAP theorem，三个只能满足两个。Amazon的例子：
  * For the checkout process you always want to honor requests to add items to a shopping cart because it's revenue producing. In this case you choose high availability. Errors are hidden from the customer and sorted out later.
  * When a customer submits an order you favor consistency because several services--credit card processing, shipping and handling, reporting--are simultaneously accessing the data.

## Lessons Learned

* 永远别share database
* 如果可能，选择“fast reboot and fast recover”的方法来解决问题
* Keep things as simple as possible
* Open a system with API is a better approach -- It will build the ecosystem.
* 永远试图保持一个STAGE环境来进行上线之前的测试
* 一个稳定可靠的文件系统对web service来说是非常重要的
* 永远为deployment失败做好准备，保证rollback方案到位
* 创新永远源自基层