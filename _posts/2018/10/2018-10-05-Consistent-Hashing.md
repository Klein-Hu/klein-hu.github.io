---
layout: post
title: Consistent Hashing
categories: [Reading]
tags: [Career, Distributed System]
fullview: false
comments: true
---

最近关注了一下Consistent Hashing这个领域，读了三篇文章。这里记载一下收获。

## 三篇文章

1. https://www.toptal.com/big-data/consistent-hashing
2. http://michaelnielsen.org/blog/consistent-hashing/
3. https://medium.com/@dgryski/consistent-hashing-algorithmic-tradeoffs-ef6b8e2fcae84. 

## 第一篇

从第一篇开始是因为它写的很详细，几乎每一步都有图。但内容只涵盖了基本Hash和Ring Hash。可以用来入门，但估计不会看第二次。

## 第二篇

从Google的架构入手讲为啥需要Consistent Hashing：这个技术是MapReduce的基础之一。

其他内容：

1. 直接Hash肯定会导致机器数量变化引起大规模数据移动；
2. 把Hash之后的数放在0-1的一个环上；机器想象成换上的锚点；Hash之后的数据是换上的某一个点，于是把这个数据放在顺时针距离最近的机器锚点上；
3. 上面的做法有可能会导致数据不均匀分布在机器之间
4. 改进的做法是把每一个机器想象成多个锚点，这样一来圆环被分了更多份，Hash上来的数据就会相对更均匀的分布在机器之间。

总起来说比第一篇要深入一些，但估计也没有啥意义再去看第二遍

## 第三篇

这篇我以后肯定还得反复看 :-)

### 基础Hash函数

* SHA-1
* MD5 (good but expensive)
* MurMurHash
* xxHash
* MetroHash
* SipHash1-3

### Ring Hash

**原始论文**：[Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web](https://www.akamai.com/es/es/multimedia/documents/technical-publication/consistent-hashing-and-random-trees-distributed-caching-protocols-for-relieving-hot-spots-on-the-world-wide-web-technical-publication.pdf)

使用这个方法的PROD级别的应用：Last.fm和Amazon Dynamo。

**不足之处**：Load distribution仍然可能不均匀，进而导致cache misses从而增加memory cost。

### Jump Hash

**原始论文**：[A Fast, Minimal Memory, Consistent Hash Algorithm](https://arxiv.org/abs/1406.2294)。

此算法的实际应用：[2011 release of the Guava libraries](https://github.com/google/guava/commit/f9318924b71d4ed2c59d3d835fe6a4ce3feefcbf#diff-e6922c1523168e608b83158728d07e63R188)

**思路**：hash(key)作为随机数种子喂给随机数发生器，用生成的随机数“jump forward”直到从离开序列。离开之前最后一个位置就被用作存储这个key-value的机器。

**优点**

1. 几乎没有memory overhead
2. 快，O(ln n)比一般的binary search的O(log n)快。而且在寄存器里就可以计算，避免了cache miss。

**缺点**

1. 只能是0 -> numBuckets-1之间的一个整数，不支持任意bucket name。
2. 也因为如此，Jump-Hash的结果跟机器序号直接相关，由此导致添加删除机器必须在两端进行。
3. 不支持机器“权重”，比如不同性能的机器。

由此，Jump-Hash更适合有replica的storage sharding，而不太适合Consistent Hashing。

### Multi-Probe Hashing

**原始论文**：[Multi-probe consistent hashing](https://arxiv.org/abs/1505.00062)

O(n)空间，添加删除节点需要O(1)时间。但lookup变慢了。

### Rendezvous Hashing

**原始论文**：[A Name-Based Mapping Scheme for Rendezvous](https://www.eecs.umich.edu/techreports/cse/96/CSE-TR-316-96.pdf)

**思路**：把key和node（机器）一起hash，取hash结果最大的那个。

**缺点**：这个算法的问题在于lookup的时候是O(n)的。

### Maglev Hashing

**原始论文**：[Maglev: A Fast and Reliable Software Network Load Balancer](https://research.google.com/pubs/pub44824.html)

**优点**：使用lookup table使得找到node需要O(1)时间。

**缺点**

1. 如果node挂了，重建新的lookup table会很慢；
2. 对node的个数有上限。

作为load balancer，这个算法应该够了。

相关论文：[Maglev: A Fast and Reliable Software Network Load Balancer](https://blog.acolyer.org/2016/03/21/maglev-a-fast-and-reliable-software-network-load-balancer/)

## 其他问题


### Replica的选择

不同的算法的做法类似：基本上就是“下一个”

* Ring hash, multi-probe就是环上下一个；
* Rendezvous就是第二大的hash result。
* Jump也可以有类似的做法。

有关的视频：[Graphite Metrics Storage at Booking.com](https://youtu.be/RzO2tmrPRfo?t=13m50s)

### 权重处理

* Ring hash是最容易处理的，多加几个virtual replica就行了。
* Rendezvous hashing只需要改变选择最大值的方式：[Weighted rendezvous hashing](http://www.snia.org/sites/default/files/SDC15_presentations/dist_sys/Jason_Resch_New_Consistent_Hashings_Rev.pdf) 。
* 而其他的都比较tricky。

### Load Balancing

用consistent hashing做load balancing的问题在于有可能效率还不如random分发来的均衡。

为了改进这个问题，有两个来自Google的做法

* [Consistent Hashing with Bounded Loads](https://research.googleblog.com/2017/04/consistent-hashing-with-bounded-loads.html)
* 另一个来自Google SRE Book: [Load Balancing in the Datacenter](https://landing.google.com/sre/book/chapters/load-balancing-datacenter.html)

基本上Load Balancing是可以单独写书的，此处作者给了两个overview:

* [Load Balancing Is Impossible](https://www.youtube.com/watch?v=kpvbOzHUakA) by Tyler McMullen
* [Predictive Load-Balancing: Unfair But Faster & More Robust](https://www.youtube.com/watch?v=6NdxUY1La2I) by Steve Gury

基本结论就是没有完美的consistent hashing算法，各种trade-off而已。




