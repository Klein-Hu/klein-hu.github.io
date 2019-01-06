---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 07
categories: [Reading]
tags: [Design, Design-pattern, Concurrency, Parallel]
fullview: false
comments: true
published: true
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - "The Lambda Architecture". The sample code is in Java.

**Lambda Architecture** is due to the deep similarities between the architecture and functional programming. At the most fundamental level, the Lambda Architecture is a general way to compute functions on all your data at once. 

**MapReduce** is one of underlying technologies of **Lambda Architecture**. **Hadoop** is the most popular **MapReduce** framework.

In Hadoop, 
  - **Mappers** take some input format (by default, lines of plain text) and map it to a number of key/value pairs.
  - **Reducers** convert these key/value pairs to the ultimate output format(normally also a set of key/value pairs).

Mappers and reducers are distributed across many different physical machines (there’s no requirement for there to be the same number of mappers as reducers). The key/value pairs from a single mapper are sent to multiple reducers. All pairs with the same key will be processed by the same reducer, no matter which mapper generated them. It is called **shuffle phase**

One thing we need to pay attention, Hadoop guarantees that keys will be sorted before being passed to a reducer, a fact that is very helpful for some tasks.

We use Hadoop as the framework of MapReduce, the reason is not only speed, also:
  - Fault-tolerant and retry for operations
  - Fault-tolerant file system (HDFS)
  - Handle data size which can't fit in memory with HDFS

Limitation of traditional data system:
  - Scaling
  - Maintainence Overhead
  - Complexity
  - Human Errors
  - Reporting and Analysis

Data moves from the operational database to the data warehouse through a process known as **E**xtract, **T**ransform, **L**oad (**ETL**).

Raw data is eternally true and therefore immutable, is the fundamental basis of the Lambda Architecture.

The primary drawback of the batch layer is *latency*, which the Lambda Architecture addresses by running the speed layer alongside.

Speeding layer, is therefore intrinsically more difficult than building the batch layer because it’s forced to take an incremental approach. The speeding layer could be sync or async. One of async solutions would be Apache Storm.

### Open Readings
1. Amazon Elastic MapReduce [EMR](http://aws.amazon.com/elasticmapreduce/) and [Developer Guide](http://docs.aws.amazon.com/ElasticMapReduce/latest/DeveloperGuide/emr-plan-hadoop-version.html)
2. [ElephantDB](https://github.com/nathanmarz/elephantdb)
3. [Project Voldemort](http://www.project-voldemort.com/voldemort/)
4. [Apache Storm](http://storm.apache.org/releases/current/Understanding-the-parallelism-of-a-Storm-topology.html)
5. [Kafka](http://kafka.apache.org/)
6. [Kestrel](http://robey.github.io/kestrel/)
7. [Trident](http://storm.apache.org/releases/current/Trident-tutorial.html)

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)