---
layout: post
title: C++ Build-in Algorithms and Data Structures - 04
categories: [Reading]
tags: [Career, C++, Languages]
fullview: false
comments: true
---

本系列其他文章：

* [C++ Build-in Algorithms and Data Structures - 01](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-01.html)
* [C++ Build-in Algorithms and Data Structures - 02](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-02.html)
* [C++ Build-in Algorithms and Data Structures - 03](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-03.html)

本文来自于[cplusplus.com reference](http://www.cplusplus.com/reference/algorithm/)。此处默认`C++`版本是`11`。不考虑各种数据结构和算法在`C++98`的表现。

通用表达见[C++ Build-in Algorithms and Data Structures - 01](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-01.html)

# 有序序列的查找和合并

## 简表

TODO

## 二分查找

* `lower_bound`
* `upper_bound`
* `equal_range`
* `binary_search`

```C++
template <class ForwardIterator, class T>
  ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last,
                               const T& val);

template <class ForwardIterator, class T, class Compare>
  ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last,
                               const T& val, Compare comp);
```

找到第一个大于等于`val`的值的位置。如果存在并且有重复，就是从左到右第一个出现的位置；如果不存在，就是第一个比`val`大的位置，也就是如果插入这个`val`，需要插入的位置。此处位置指的是iterator，不是序号。

```C++
template <class ForwardIterator, class T>
  ForwardIterator upper_bound (ForwardIterator first, ForwardIterator last,
                               const T& val);

template <class ForwardIterator, class T, class Compare>
  ForwardIterator upper_bound (ForwardIterator first, ForwardIterator last,
                               const T& val, Compare comp);
```

找到第一个大于`val`的值的位置。此处位置指的是iterator，不是序号。


```C++
template <class ForwardIterator, class T>
  pair<ForwardIterator,ForwardIterator>
    equal_range (ForwardIterator first, ForwardIterator last, const T& val);

template <class ForwardIterator, class T, class Compare>
  pair<ForwardIterator,ForwardIterator>
    equal_range (ForwardIterator first, ForwardIterator last, const T& val,
                  Compare comp);
```

返回序列中等于`val`的值的范围，[first, last)。此处也都是iterator，不是序号。

```C++
template <class ForwardIterator, class T>
  bool binary_search (ForwardIterator first, ForwardIterator last,
                      const T& val);

template <class ForwardIterator, class T, class Compare>
  bool binary_search (ForwardIterator first, ForwardIterator last,
                      const T& val, Compare comp);
```

找到就返回`true`，找不到返回`false`，至于找到以后在什么位置，不知道。看上去没啥用，不如直接用`lower_bound`然后判断返回值的位置以及指向的值是不是等于`val`就得了。

