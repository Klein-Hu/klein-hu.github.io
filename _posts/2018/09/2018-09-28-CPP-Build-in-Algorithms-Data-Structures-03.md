---
layout: post
title: C++ Build-in Algorithms and Data Structures - 03
categories: [Reading]
tags: [Career, C++, Languages]
fullview: false
comments: true
---

本系列其他文章：

* [C++ Build-in Algorithms and Data Structures - 01](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-01.html)
* [C++ Build-in Algorithms and Data Structures - 02](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-02.html)

本文来自于[cplusplus.com reference](http://www.cplusplus.com/reference/algorithm/)。此处默认`C++`版本是`11`。不考虑各种数据结构和算法在`C++98`的表现。

通用表达见[C++ Build-in Algorithms and Data Structures - 01](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-01.html)

# 排序相关

## 简表

TODO

## Partitions 分区操作

* `is_partitioned`
* `partition`
* `stable_partition`
* `partition_copy`
* `partition_point`

```C++
template <class InputIterator, class UnaryPredicate>
  bool is_partitioned (InputIterator first, InputIterator last, UnaryPredicate pred);
```

当列表中所有满足`pred`为`true`的值都在`pred`为`false`的值之前出现的时候，该函数返回`true`。

```C++
template <class ForwardIterator, class UnaryPredicate>
  ForwardIterator partition (ForwardIterator first,
                             ForwardIterator last, UnaryPredicate pred);
```

返回值指向partition之后的后面那半部分的第一个元素，如果没有第二部分，则指向`end()`

```C++
template <class BidirectionalIterator, class UnaryPredicate>
  BidirectionalIterator stable_partition (BidirectionalIterator first,
                                          BidirectionalIterator last,
                                          UnaryPredicate pred);
```