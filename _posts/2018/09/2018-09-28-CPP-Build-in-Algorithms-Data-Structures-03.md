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

|                     | **功能**                                                                   |
|---------------------|------------------------------------------------------------------------|
| **分区操作**            |                                                                        |
| `is_partitioned`    | 判断是不是已经分区                                                     |
| `partition`         | 执行分区操作                                                           |
| `stable_partition`  | 类似分区，但保持原始相对位置                                           |
| `partition_copy`    | 拷贝过程中分区，拷贝的目标位置是分好区的，原始位置不动                 |
| `partition_point`   | 分区，然后返回后一个分区的开始位置                                     |
| **排序操作**            |                                                                        |
| `sort`              | 快速排序                                                               |
| `stable_sort`       | 稳定快速排序                                                           |
| `partial_sort`      | 中间位置的左边都比中间值小，右边都比中间值大                           |
| `partial_sort_copy` | 排序之后的内容拷贝到新位置，如果新位置地方小就只保留前面能放的下的那些 |
| `is_sorted`         | 判断是不是已经排序了                                                   |
| `is_sorted_until`   | 返回第一个没有排序的元素的位置                                         |
| `nth_element`       | 找到第n小的元素，左边都比它小，右边都比它大。                          |

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

返回值指向partition之后的后面那`partition_point`半部分的第一个元素，如果没有第二部分，则指向`end()`

```C++
template <class BidirectionalIterator, class UnaryPredicate>
  BidirectionalIterator stable_partition (BidirectionalIterator first,
                                          BidirectionalIterator last,
                                          UnaryPredicate pred);
```

跟`partition`类似，但保持原有顺序。

```C++
template <class InputIterator, class OutputIterator1,
          class OutputIterator2, class UnaryPredicate pred>
  pair<OutputIterator1,OutputIterator2>
    partition_copy (InputIterator first, InputIterator last,
                    OutputIterator1 result_true, OutputIterator2 result_false,
                    UnaryPredicate pred);
```

把满足`pred == true`的拷贝到`result_true`开始的位置，不满足的拷贝到`result_false`开始的位置。返回值是个pair，`first`是满足`pred==true`的序列的末尾的下一个位置，`second`是不满足序列的末尾`partition_point`个位置。

```C++
template <class ForwardIter`partition_point` class UnaryPredicate>
  ForwardIterator partition_point (ForwardIterator first, ForwardIterator last,
                                   UnaryPredicate pred);
```

先分区，然后返回第一个`pred==false`的位置，或者`last`。

## Sorting 排序操作

* `sort`
* `stable_sort`
* `partial_sort`
* `partial_sort_copy`
* `is_sorted`
* `is_sorted_until`
* `nth_element`

```C++
template <class RandomAccessIterator>
  void sort (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

`comp`可以是函数指针，也可以是函数对象。下同。

```C++
template <class RandomAccessIterator>
  void stable_sort ( RandomAccessIterator first, RandomAccessIterator last );

template <class RandomAccessIterator, class Compare>
  void stable_sort ( RandomAccessIterator first, RandomAccessIterator last,
                     Compare comp );
```

类似`sort`，但保证相对位置。

```C++
template <class RandomAccessIterator>
  void partial_sort (RandomAccessIterator first, RandomAccessIterator middle,
                     RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void partial_sort (RandomAccessIterator first, RandomAccessIterator middle,
                     RandomAccessIterator last, Compare comp);
```

`middle`左边的升序并且比`middle`指向的元素小，`middle`右边的元素无序。`comp`是自定义的`operator<`。

```C++
template <class InputIterator, class RandomAccessIterator>
  RandomAccessIterator
    partial_sort_copy (InputIterator first,InputIterator last,
                       RandomAccessIterator result_first,
                       RandomAccessIterator result_last);

template <class InputIterator, class RandomAccessIterator, class Compare>
  RandomAccessIterator
    partial_sort_copy (InputIterator first,InputIterator last,
                       RandomAccessIterator result_first,
                       RandomAccessIterator result_last, Compare comp);
```

把原序列的数据排序好拷贝到新位置。如果新位置空间比原来少，装满为止；如果新位置空间比原来少，则拷贝完原有的数据就停了。原位置数据不变。返回值是新位置数据末尾的下一个位置，可以是`result_last`。

```C++
template <class ForwardIterator>
  bool is_sorted (ForwardIterator first, ForwardIterator last);

template <class ForwardIterator, class Compare>
  bool is_sorted (ForwardIterator first, ForwardIterator last, Compare comp);
```

返回`true`，如果序列在`operator<`或者`comp`下是升序。

```C++
template <class ForwardIterator>
  ForwardIterator is_sorted_until (ForwardIterator first, ForwardIterator last);

template <class ForwardIterator, class Compare>
  ForwardIterator is_sorted_until (ForwardIterator first, ForwardIterator last,
                                   Compare comp);
```

返回一个iterator，使得`first`到这个位置是对于`operator<`或者`comp`升序。

```C++
template <class RandomAccessIterator>
  void nth_element (RandomAccessIterator first, RandomAccessIterator nth,
                    RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void nth_element (RandomAccessIterator first, RandomAccessIterator nth,
                    RandomAccessIterator last, Compare comp);
```

指定一个nth element位置，然后这个函数让这个位置是第n小的元素，左边的都比这个元素小，右边的都比它大。