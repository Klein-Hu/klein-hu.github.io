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

本文来自于[cplusplus.com reference](http://www.cplusplus.com/reference/algorithm/)。此处默认`C++`版本是`11`或更高版本。不考虑各种数据结构和算法在`C++98`的表现。

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


## 合并有序序列

* `merge`
* `inplace_merge`
* `includes`
* `set_union`
* `set_intersection`
* `set_difference`
* `set_symmetric_difference`

```C++
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                        InputIterator2 first2, InputIterator2 last2,
                        OutputIterator result);

template <class InputIterator1, class InputIterator2, class OutputIterator, class Compare>
  OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                        InputIterator2 first2, InputIterator2 last2,
                        OutputIterator result, Compare comp);
```

两个有序序列`InputIterator1`，`InputIterator2`合并到新的有序序列`OutputIterator`。

```C++
template <class BidirectionalIterator>
  void inplace_merge (BidirectionalIterator first, BidirectionalIterator middle,
                      BidirectionalIterator last);

template <class BidirectionalIterator, class Compare>
  void inplace_merge (BidirectionalIterator first, BidirectionalIterator middle,
                      BidirectionalIterator last, Compare comp);
```

把`[first,middle)`和`[middle,last)`合并成`[first,last)`。整个过程in-place。

```C++
template <class InputIterator1, class InputIterator2>
  bool includes ( InputIterator1 first1, InputIterator1 last1,
                  InputIterator2 first2, InputIterator2 last2 );

template <class InputIterator1, class InputIterator2, class Compare>
  bool includes ( InputIterator1 first1, InputIterator1 last1,
                  InputIterator2 first2, InputIterator2 last2, Compare comp );
```

看看是不是`[first1,last1)`包含所有的`[first2,last2)`

```C++
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_union (InputIterator1 first1, InputIterator1 last1,
                            InputIterator2 first2, InputIterator2 last2,
                            OutputIterator result);

template <class InputIterator1, class InputIterator2,
          class OutputIterator, class Compare>
  OutputIterator set_union (InputIterator1 first1, InputIterator1 last1,
                            InputIterator2 first2, InputIterator2 last2,
                            OutputIterator result, Compare comp);
```

对于两个有序序列，得到两个有序序列的并集。返回值是the end of result。

```C++
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_intersection (InputIterator1 first1, InputIterator1 last1,
                                   InputIterator2 first2, InputIterator2 last2,
                                   OutputIterator result);

template <class InputIterator1, class InputIterator2,
          class OutputIterator, class Compare>
  OutputIterator set_intersection (InputIterator1 first1, InputIterator1 last1,
                                   InputIterator2 first2, InputIterator2 last2,
                                   OutputIterator result, Compare comp);
```

对于两个有序序列，得到两个有序序列的交集。返回值是the end of result。

```C++
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_difference (InputIterator1 first1, InputIterator1 last1,
                                 InputIterator2 first2, InputIterator2 last2,
                                 OutputIterator result);

template <class InputIterator1, class InputIterator2,
          class OutputIterator, class Compare>
  OutputIterator set_difference (InputIterator1 first1, InputIterator1 last1,
                                 InputIterator2 first2, InputIterator2 last2,
                                 OutputIterator result, Compare comp);
```

对于两个有序序列，得到所有在`[first1,last1)`但不在`[first2,last2)`中的元素——差集。返回值是the end of result。

```C++
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_symmetric_difference (InputIterator1 first1, InputIterator1 last1,
                                           InputIterator2 first2, InputIterator2 last2,
                                           OutputIterator result);

template <class InputIterator1, class InputIterator2,
          class OutputIterator, class Compare>
  OutputIterator set_symmetric_difference (InputIterator1 first1, InputIterator1 last1,
                                           InputIterator2 first2, InputIterator2 last2,
                                           OutputIterator result, Compare comp);
```

对于两个有序序列，得到所有在并集中但不在交集中的元素——对称差集。返回值是the end of result。

## 堆

* `push_heap`
* `pop_heap`
* `make_heap`
* `sort_heap`
* `is_heap`
* `is_heap_until`

```C++
template <class RandomAccessIterator>
  void push_heap (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void push_heap (RandomAccessIterator first, RandomAccessIterator last,
                   Compare comp);
```

已有序列`[first,last)`的`[first,last-1)`部分是heap，此函数把调整后把`[first,last)`变成heap。

```C++
template <class RandomAccessIterator>
  void pop_heap (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void pop_heap (RandomAccessIterator first, RandomAccessIterator last,
                 Compare comp);
```

已有序列`[first,last)`是heap，`pop`之后原来第一个元素到了最后，`[first,last-1)`仍然是heap。

```C++
template <class RandomAccessIterator>
  void make_heap (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void make_heap (RandomAccessIterator first, RandomAccessIterator last,
                  Compare comp );
```

创建heap

```C++
template <class RandomAccessIterator>
  void sort_heap (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  void sort_heap (RandomAccessIterator first, RandomAccessIterator last,
                  Compare comp);
```

已有序列`[first,last)`是heap，此函数将其进行排序，使得结果是升序。

```C++
template <class RandomAccessIterator>
  bool is_heap (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  bool is_heap (RandomAccessIterator first, RandomAccessIterator last,
                Compare comp);
```

检查`[first,last)`是不是heap

```C++
template <class RandomAccessIterator>
  RandomAccessIterator is_heap_until (RandomAccessIterator first,
                                      RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
  RandomAccessIterator is_heap_until (RandomAccessIterator first,
                                      RandomAccessIterator last
                                      Compare comp);
```

找到不满足heap条件的第一个element。如果`[first,last)`是heap则返回`last`

## 最大最小值

* `min`
* `max`
* `minmax`
* `min_element`
* `max_element`
* `minmax_element`

```C++14
template <class T> constexpr const T& min (const T& a, const T& b);

template <class T, class Compare>
  constexpr const T& min (const T& a, const T& b, Compare comp);

template <class T> constexpr T min (initializer_list<T> il);

template <class T, class Compare>
  constexpr T min (initializer_list<T> il, Compare comp);
```

返回最小值。如果两个值一样，返回`a`

```C++14
template <class T> constexpr const T& max (const T& a, const T& b);

template <class T, class Compare>
  constexpr const T& max (const T& a, const T& b, Compare comp);

template <class T> constexpr T max (initializer_list<T> il);

template <class T, class Compare>
  constexpr T max (initializer_list<T> il, Compare comp);
```

返回最大值。如果两个值一样，返回`a`

```C++14
template <class T>
  constexpr pair <const T&,const T&> minmax (const T& a, const T& b);
	
template <class T, class Compare>
  constexpr pair <const T&,const T&> minmax (const T& a, const T& b, Compare comp);

template <class T>
  constexpr pair<T,T> minmax (initializer_list<T> il);

template <class T, class Compare>
  constexpr pair<T,T> minmax (initializer_list<T> il, Compare comp);
```

返回最小值和最大值组成的`pair`，`<min_val, max_val>`。

```C++
template <class ForwardIterator>
  ForwardIterator min_element (ForwardIterator first, ForwardIterator last);
	
template <class ForwardIterator, class Compare>
  ForwardIterator min_element (ForwardIterator first, ForwardIterator last,
                               Compare comp);
```

返回指向最小值的iterator。如果多个元素都满足条件，返回指向第一个的iterator。

```C++
template <class ForwardIterator>
  ForwardIterator max_element (ForwardIterator first, ForwardIterator last);
	
template <class ForwardIterator, class Compare>
  ForwardIterator max_element (ForwardIterator first, ForwardIterator last,
                               Compare comp);
```

返回指向最大值的iterator。如果多个元素都满足条件，返回指向第一个的iterator。


```C++
template <class ForwardIterator>
  pair<ForwardIterator,ForwardIterator>
    minmax_element (ForwardIterator first, ForwardIterator last);
	
template <class ForwardIterator, class Compare>
  pair<ForwardIterator,ForwardIterator>
    minmax_element (ForwardIterator first, ForwardIterator last, Compare comp);
```

返回指向最小值和指向最大值组成的`pair`，`<min_val_iter, max_val_iter>`。

## 其他

* `lexicographical_compare`
* `next_permutation`
* `prev_permutation`

```C++
template <class InputIterator1, class InputIterator2>
  bool lexicographical_compare (InputIterator1 first1, InputIterator1 last1,
                                InputIterator2 first2, InputIterator2 last2);
	
template <class InputIterator1, class InputIterator2, class Compare>
  bool lexicographical_compare (InputIterator1 first1, InputIterator1 last1,
                                InputIterator2 first2, InputIterator2 last2,
                                Compare comp);
```

[字典序](https://en.wikipedia.org/wiki/Lexicographical_order)比较：不考虑长度，一个一个字母从左到右比较，当出现不一样的字符时，以这两个不一样字符的比较结果决定整个字符串的比较结果。

```C++
template <class BidirectionalIterator>
  bool next_permutation (BidirectionalIterator first,
                         BidirectionalIterator last);
	
template <class BidirectionalIterator, class Compare>
  bool next_permutation (BidirectionalIterator first,
                         BidirectionalIterator last, Compare comp);
```

按照[字典序](https://en.wikipedia.org/wiki/Lexicographical_order)对序列找到下一个排列，in-place改变这个序列。如果成功，返回`true`；如果失败返回`false`。但`false`的时候可能`prev_permutation`能够正常工作。

```C++
template <class BidirectionalIterator>
  bool prev_permutation (BidirectionalIterator first,
                         BidirectionalIterator last );
	
template <class BidirectionalIterator, class Compare>
  bool prev_permutation (BidirectionalIterator first,
                         BidirectionalIterator last, Compare comp);
```

按照[字典序](https://en.wikipedia.org/wiki/Lexicographical_order)对序列找到下一个排列，in-place改变这个序列。如果成功，返回`true`；如果失败返回`false`。但`false`的时候可能`next_permutation`能够正常工作。

