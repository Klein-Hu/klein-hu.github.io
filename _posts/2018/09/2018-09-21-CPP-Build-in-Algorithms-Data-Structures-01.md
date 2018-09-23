---
layout: post
title: C++ Build-in Algorithms and Data Structures - 01
categories: [Reading]
tags: [Career, C++, Languages]
fullview: false
comments: true
---

刷题中发现C++其实有很多build-in的算法和数据结构是非常好用的，比如[LeetCode 295](https://leetcode.com/problems/find-median-from-data-stream/)，直接用`priority_queue`就能AC，直接用`vector` + `make_heap()`会TLE。于是决定静下心了读文档，整理一份笔记，有关C++ build-in的数据结构和算法。

本文来自于[cplusplus.com reference](http://www.cplusplus.com/reference/algorithm/)。此处默认`C++`版本是`11`。不考虑各种数据结构和算法在`C++98`的表现。

# 通用表达

* `UnaryPredicate`：一元表达式，输入是一个range中的元素，输出是bool。可以是函数指针或者函数对象，比如struct里面有个叫operator()(T input)的函数。这个函数指针或者operator()(T)接受的数据类型就是前面iterator指向的。
* `BinaryPredicate`：二元表达式，类似一元表达式，但多数情况下两个元素来自于不同的range里。输出是`bool`。同样可以是函数指针或者函数对象。
* `ForwardIterator`：单向iterator，只能从begin --> end。实际上`BidirectionalIterator`和`RandomIterator`同样也可以用作`ForwardIterator`。
* `Generator`：没有输入arguments，返回一个跟当前范围类型一致的值。同样可以是函数指针或者函数对象。

# Sequence 只读操作

**注意**：以下操作均为只读操作，任何lambda或者函数指针都不能修改数据

## 简表

|                | 功能                                                                                                    | 复杂度        |
|----------------|---------------------------------------------------------------------------------------------------------|---------------|
| **存在性判断**     |                                                                                                         |               |
| `all_of`         | 是不是所有元素都满足条件                                                                                | O(n)          |
| `any_of`         | 是不是有元素满足条件                                                                                    | O(n)          |
| `none_of`        | 是不是不存在这样的元素                                                                                  | O(n)          |
| **对每个元素访问** |                                                                                                         |               |
| `for_each`       | 对每个元素进行只读访问                                                                                  | O(n)          |
| **查找**           |                                                                                                         |               |
| `find`           | 简单找元素                                                                                              | O(n)          |
| `find_if`        | 按条件找元素                                                                                            | O(n)          |
| `find_if_not`    | 按反向条件找元素                                                                                        | O(n)          |
| `find_end`       | 在`[first1, last1)`里面找`[first2, last2)`最后出现的位置。                                              | O(m*n)        |
| `find_first_of`  | 在`[first1, last1)`里面找`[first2, last2)`里任何一个元素在`[first1, last1)`第一次出现的位置。           | O(m*n)        |
| `adjacent_find`  | 在`[first1, last1)`里面找第一次出现的连续两个元素相同的情况。                                           | O(n)          |
| **计数**           |                                                                                                         |               |
| `count`          | 对`[first1, last1)`中所有`val`的个数进行计数                                                            | O(n)          |
| `count_if`       | 有条件计数，对所有`pred`是`true`的情况计数。                                                            | O(n)          |
| **对两个范围比较** |                                                                                                         |               |
| `mismatch`       | 把`[first1, last1)`中每一个元素在`first2`开始的范围内进行比较，返回一个`pair`，表明第一个mismatch的地方 | O(n)          |
| `equal`          | 判断两个range内的元素是不是都一样。                                                                     | O(n)          |
| `is_permutation` | 判断一个range是不是另一个range里元素的另一种排列。                                                      | O(n) → O(n^2) |
| `search`         | 在`[first1, last1)`里面找`[first2, last2)`第一次出现的位置。                                            | O(m*n)        |
| `search_n`       | 在`[first1, last1)`里面找`val`连续出现`count`次的位置。                                                 | O(n)          |

## 存在性判断

* `all_of`
* `any_of`
* `none_of`

```C++
template<class InputIterator, class UnaryPredicate>
  bool all_of (InputIterator first, InputIterator last, UnaryPredicate pred)
```

```C++
template<class InputIterator, class UnaryPredicate>
  bool any_of (InputIterator first, InputIterator last, UnaryPredicate pred)
```

```C++
template<class InputIterator, class UnaryPredicate>
  bool none_of (InputIterator first, InputIterator last, UnaryPredicate pred)
```

最后一个参数是可以使lambda表达式，见“通用表达”。

## 对每个元素进行特定只读操作

* `for_each`

```C++
template<class InputIterator, class Function>
  Function for_each(InputIterator first, InputIterator last, Function fn)
```

最后一个参数是可以使lambda表达式，见“通用表达”。此函数如果有返回值将会被忽略。

## 查找

任何情况下，找不到的时候返回“长”的range的`last`。

* `find`
* `find_if`
* `find_if_not`
* `find_end`
* `find_first_of`
* `adjacent_find`

```C++
template<class InputIterator, class T>
  InputIterator find (InputIterator first, InputIterator last, const T& val)
```

简单查找，使用`T`自己的`==`操作来进行比较。注意最后一个参数类型是`const T&`。

```C++
template<class InputIterator, class UnaryPredicate>
  InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred)
```

最后一个参数是可以使lambda表达式，见“通用表达”。

```C++
template<class InputIterator, class UnaryPredicate>
  InputIterator find_if_not (InputIterator first, InputIterator last, UnaryPredicate pred)
```

最后一个参数是可以使lambda表达式，见“通用表达”。

```C++
template <class ForwardIterator1, class ForwardIterator2>
   ForwardIterator1 find_end (ForwardIterator1 first1, ForwardIterator1 last1,
                              ForwardIterator2 first2, ForwardIterator2 last2);

template <class ForwardIterator1, class ForwardIterator2, class BinaryPredicate>
   ForwardIterator1 find_end (ForwardIterator1 first1, ForwardIterator1 last1,
                              ForwardIterator2 first2, ForwardIterator2 last2,
                              BinaryPredicate pred);
```

在`[first1, last1)`里面找`[first2, last2)`最后出现的位置。`pred`可以用来指定什么叫做“match”，默认的情况下使用`operator==`来做判断。

```C++
template <class InputIterator, class ForwardIterator>
   InputIterator find_first_of (InputIterator first1, InputIterator last1,
                                   ForwardIterator first2, ForwardIterator last2);

template <class InputIterator, class ForwardIterator, class BinaryPredicate>
   InputIterator find_first_of (InputIterator first1, InputIterator last1,
                                   ForwardIterator first2, ForwardIterator last2,
                                   BinaryPredicate pred);
```

在`[first1, last1)`里面找`[first2, last2)`里任何一个元素在`[first1, last1)`第一次出现的位置。`pred`可以用来指定什么叫做“match”，默认的情况下使用`operator==`来做判断。

```C++
template <class ForwardIterator>
   ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last);

template <class ForwardIterator, class BinaryPredicate>
   ForwardIterator adjacent_find (ForwardIterator first, ForwardIterator last,
                                  BinaryPredicate pred);
```

在`[first1, last1)`里面找第一次出现的连续两个元素相同的情况。返回的是连续两个元素中前面那个的iterator。

## 计数

* count
* count_if

```C++
template <class InputIterator, class T>
  typename iterator_traits<InputIterator>::difference_type
    count (InputIterator first, InputIterator last, const T& val);
```

对`[first1, last1)`中所有`val`的个数进行计数，使用`operator==`来判定相等。

```C++
template <class InputIterator, class UnaryPredicate>
  typename iterator_traits<InputIterator>::difference_type
    count_if (InputIterator first, InputIterator last, UnaryPredicate pred);
```

有条件计数，对所有`pred`是`true`的情况计数。

## 对两个范围比较

* `mismatch`
* `equal`
* `is_permutation`
* `search`
* `search_n`

```C++
template <class InputIterator1, class InputIterator2>
  pair<InputIterator1, InputIterator2>
    mismatch (InputIterator1 first1, InputIterator1 last1,
              InputIterator2 first2);

template <class InputIterator1, class InputIterator2, class BinaryPredicate>
  pair<InputIterator1, InputIterator2>
    mismatch (InputIterator1 first1, InputIterator1 last1,
              InputIterator2 first2, BinaryPredicate pred);
```

把`[first1, last1)`中每一个元素在`first2`开始的范围内进行比较，返回一个`pair`：

* 如果找到了mismatch，就是各自的iterator位置
* 如果没有mismatch，就是`(last1, something)`；something是当时第二个范围内的iterator的位置。
* 如果上来就不一样，直接返回`(first1, first2)`

```C++
template <class InputIterator1, class InputIterator2>
  bool equal (InputIterator1 first1, InputIterator1 last1,
              InputIterator2 first2);

template <class InputIterator1, class InputIterator2, class BinaryPredicate>
  bool equal (InputIterator1 first1, InputIterator1 last1,
              InputIterator2 first2, BinaryPredicate pred);
```

判断两个range内的元素是不是都一样。

```C++
template <class ForwardIterator1, class ForwardIterator2>
   bool is_permutation (ForwardIterator1 first1, ForwardIterator1 last1,
                        ForwardIterator2 first2);

template <class ForwardIterator1, class ForwardIterator2, class BinaryPredicate>
   bool is_permutation (ForwardIterator1 first1, ForwardIterator1 last1,
                        ForwardIterator2 first2, BinaryPredicate pred);
```

判断一个range是不是另一个range里元素的另一种排列。元素的比较使用`operator==`或者`pred`。如果两个range完全相同，也会返回`true`。这个函数的[实现](http://www.cplusplus.com/reference/algorithm/is_permutation/)很漂亮。

```C++
template <class ForwardIterator1, class ForwardIterator2>
   ForwardIterator1 search (ForwardIterator1 first1, ForwardIterator1 last1,
                            ForwardIterator2 first2, ForwardIterator2 last2);

template <class ForwardIterator1, class ForwardIterator2, class BinaryPredicate>
   ForwardIterator1 search (ForwardIterator1 first1, ForwardIterator1 last1,
                            ForwardIterator2 first2, ForwardIterator2 last2,
                            BinaryPredicate pred);
```

在`[first1, last1)`里面找`[first2, last2)`第一次出现的位置。如果`[first2, last2)`是空的，返回`first1`。

```C++
template <class ForwardIterator, class Size, class T>
   ForwardIterator search_n (ForwardIterator first, ForwardIterator last,
                             Size count, const T& val);

template <class ForwardIterator, class Size, class T, class BinaryPredicate>
   ForwardIterator search_n ( ForwardIterator first, ForwardIterator last,
                              Size count, const T& val, BinaryPredicate pred );
```

在`[first1, last1)`里面找`val`连续出现`count`次的位置。