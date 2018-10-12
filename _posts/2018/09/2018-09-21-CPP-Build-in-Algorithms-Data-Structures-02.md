---
layout: post
title: C++ Build-in Algorithms and Data Structures - 02
categories: [Reading]
tags: [Career, C++, Languages]
fullview: false
comments: true
---

本系列其他文章：

* [C++ Build-in Algorithms and Data Structures - 01](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-01.html)
* [C++ Build-in Algorithms and Data Structures - 03](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-03.html)
* [C++ Build-in Algorithms and Data Structures - 04](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-04.html)

本文来自于[cplusplus.com reference](http://www.cplusplus.com/reference/algorithm/)。此处默认`C++`版本是`11`。不考虑各种数据结构和算法在`C++98`的表现。

通用表达见[C++ Build-in Algorithms and Data Structures - 01](https://klein-hu.github.io//reading/2018/09/21/CPP-Build-in-Algorithms-Data-Structures-01.html)

# Sequence 修改操作

## 简表

注：所有的函数都是O(n)的，所以就不再一一列出。

|                                    | 功能                                                                                                                         |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| **拷贝**                               |                                                                                                                              |
| `copy`                             | 拷贝sequence。返回新range的新的`end`位置。                                                                                   |
| `copy_n`                           | 拷贝sequence，只拷贝`n`个元素。返回新range的新的`end`位置。                                                                  |
| `copy_if`                          | 只拷贝满足`pred`条件的元素。返回新range的新的`end`位置。                                                                     |
| `copy_backward`                    | 反向拷贝到以`result`结束的位置                                                                                               |
| **移动**                               |                                                                                                                              |
| `move`                             | 类似`copy`但此处移动元素                                                                                                     |
| `move_backward`                    | 类似`copy_backward`但此处移动元素                                                                                            |
| **交换**                               |                                                                                                                              |
| `swap`                             | 可以交换两个变量，也可以交换两个容器。交换容器的时候只要类型一致即可，大小不必须一样。                                       |
| `swap_range`                       | 把`[first, last)`里面每一个元素按顺序跟`first2`开始的区域进行交换。返回第二个sequence的新的`end`位置。                       |
| `iter_swap`                        | 交换两个iterator指向的object的值。`swap`交换的两个变量的值。                                                                 |
| **对每个元素进行特定操作写入新的序列** |                                                                                                                              |
| `transform`                        | 输入可以单一序列或者两个序列，对每个元素执行`op`或者`binary_op`，写入输出序列。返回新range的`end`位置。                      |
| **替换**                               |                                                                                                                              |
| `replace`                          | 在`[first, last)`之间把所有的`old_value`替换成`new_value`。判定相等的方式只有`operator==`                                    |
| `replace_if`                       | 在`[first, last)`之间把所有满足`pred`的值都替换成`new_value`。                                                               |
| `replace_copy`                     | 把`[first, last)`之间的元素拷贝到`result`的过程中，遇到`old_value`则向`result`中写入`new_value`。返回新range的`end`位置。    |
| `replace_copy_if`                  | 把`[first, last)`之间的元素拷贝到`result`的过程中，遇到元素满足`pred`则向`result`中写入`new_value`。返回新range的`end`位置。 |
| **填充**                               |                                                                                                                              |
| `fill`                             | 给一个范围填充同一个值。                                                                                                     |
| `fill_n`                           | 给从`first`开始的区域填充`n`个`val`。返回新range的`end`位置。                                                                |
| **生成数据**                           |                                                                                                                              |
| `generate`                         | 从`first`到`last`，调用`gen`生成值并填充进去                                                                                 |
| `generate_n`                       | 从`first`开始，调用`n`次`gen`生成数据并写入。返回新range的`end`位置。                                                        |
| **删除**                               |                                                                                                                              |
| `remove`                           | 删除`[first,   last)`中所有的值为`val`的元素。返回新的`end`。                                                                |
| `remove_if`                        | 删除`[first,   last)`中所有满足`pred`的元素。返回新的`end`。                                                                 |
| `remove_copy`                      | 将`[first, last)`拷贝到`result`开始的新range过程中，忽略所有值为`val`的元素。                                                |
| `remove_copy_if`                   | 将`[first, last)`拷贝到`result`开始的新range过程中，忽略所有满足`pred`的元素。                                               |
| **去重**                               |                                                                                                                              |
| `unique`                           | 去掉sequence里的连续重复，对于任何一个连续重复，只保留第一个element。**并不是去掉所有的重复元素**。                          |
| `unique_copy`                      | 将`[first, last)`拷贝到`result`开始的新range过程中，忽略所有连续满足`operator==`的重复元素或者满足`pred`的连续重复元素。     |
| **反转**                               |                                                                                                                              |
| `reverse`                          | 将`[first, last)`范围内前后位置反转。内部调用了`iter_swap`。                                                                 |
| `reverse_copy`                     | 将`[first, last)`范围拷贝到`result`开始的新位置，但元素顺序反转。                                                            |
| **平移**                               |                                                                                                                              |
| `rotate`                           | 效果类似汇编语言循环移位操作。返回值是原来的`first`。`middle`不是`[first,   last)`范围的中位，而是中间某个位置。             |
| `rotate_copy`                      | 类似`rotate`，但实现的循环移位是在以`result`为开始的新range。                                                                |
| **洗牌**                               |                                                                                                                              |
| `shuffle`                          | 使用URNG(Uniform Random Number Generator)生成随机位置的shuffle算法。                                                         |
| `random_shuffle`                   | 使用内建的随机数发生器来产生随机位置或是有用户指定的随机数发生器来生成随即位置。                                             |

## 拷贝

* `copy`
* `copy_n`
* `copy_if`
* `copy_backward`

```C++
template <class InputIterator, class OutputIterator>
  OutputIterator copy (InputIterator first, InputIterator last, OutputIterator result);
```

`OutputIterator`不能指向`[first, last)`范围内任何一处。返回新range的新的`end`位置。

```C++
template <class InputIterator, class Size, class OutputIterator>
  OutputIterator copy_n (InputIterator first, Size n, OutputIterator result);
```

`OutputIterator`不能指向`[first, last)`范围内任何一处。返回新range的新的`end`位置。

```C++
template <class InputIterator, class OutputIterator, class UnaryPredicate>
  OutputIterator copy_if (InputIterator first, InputIterator last,
                          OutputIterator result, UnaryPredicate pred);
```

只拷贝满足`pred`条件的元素。返回新range的新的`end`位置。

```C++
template <class BidirectionalIterator1, class BidirectionalIterator2>
  BidirectionalIterator2 copy_backward (BidirectionalIterator1 first,
                                        BidirectionalIterator1 last,
                                        BidirectionalIterator2 result);
```

此处如果从`result`backward的往回拷贝时，进入和原有的`[first, last)`的区域，原有内容将被覆盖。返回新range的新的`first`位置。[此处](http://www.cplusplus.com/reference/algorithm/copy_backward/)有示例。

## 移动

* `move`
* `move_backward`

```C++
template <class InputIterator, class OutputIterator>
  OutputIterator move (InputIterator first, InputIterator last, OutputIterator result);
```

类似`copy`，src和desc的区域不能有重叠。返回新range的新的`end`位置。

```C++
template <class BidirectionalIterator1, class BidirectionalIterator2>
  BidirectionalIterator2 move_backward (BidirectionalIterator1 first,
                                        BidirectionalIterator1 last,
                                        BidirectionalIterator2 result);
```

类似`copy_backward`，从`*(last-1)`移动到`*(result-1)`开始，从后向前移动。返回新range的新的`first`位置。

## 交换

* `swap`
* `swap_range`
* `iter_swap`

```C++
template <class T> void swap (T& a, T& b)
  noexcept (is_nothrow_move_constructible<T>::value && is_nothrow_move_assignable<T>::value);

template <class T, size_t N> void swap(T (&a)[N], T (&b)[N])
  noexcept (noexcept(swap(*a,*b)));
```

从C++11起，被移动到了`<utility>`里面，但因为跟其他的两个相关，就放在这里了。可以交换两个变量，也可以交换两个容器。交换容器的时候只要类型一致即可，大小不必须一样。

```C++
template <class ForwardIterator1, class ForwardIterator2>
  ForwardIterator2 swap_ranges (ForwardIterator1 first1, ForwardIterator1 last1,
                                ForwardIterator2 first2);
```

把`[first, last)`里面每一个元素按顺序跟`first2`开始的区域进行交换。返回第二个sequence的新的`end`位置。

```C++
template <class ForwardIterator1, class ForwardIterator2>
  void iter_swap (ForwardIterator1 a, ForwardIterator2 b);
```

交换两个iterator指向的object的值。`swap`交换的两个变量的值。

## 对每个元素进行特定操作写入新的序列

* `transform`

```C++
template <class InputIterator, class OutputIterator, class UnaryOperation>
  OutputIterator transform (InputIterator first1, InputIterator last1,
                            OutputIterator result, UnaryOperation op);

template <class InputIterator1, class InputIterator2,
          class OutputIterator, class BinaryOperation>
  OutputIterator transform (InputIterator1 first1, InputIterator1 last1,
                            InputIterator2 first2, OutputIterator result,
                            BinaryOperation binary_op);
```

输入可以单一序列或者两个序列，对每个元素执行`op`或者`binary_op`，写入输出序列。返回新range的`end`位置。

## 替换

* `replace`
* `replace_if`
* `replace_copy`
* `replace_copy_if`

```C++
template <class ForwardIterator, class T>
  void replace (ForwardIterator first, ForwardIterator last,
                const T& old_value, const T& new_value);
```

在`[first, last)`之间把所有的`old_value`替换成`new_value`。判定相等的方式只有`operator==`

```C++
template <class ForwardIterator, class UnaryPredicate, class T>
  void replace_if (ForwardIterator first, ForwardIterator last,
                   UnaryPredicate pred, const T& new_value );
```

在`[first, last)`之间把所有满足`pred`的值都替换成`new_value`。

```C++
template <class InputIterator, class OutputIterator, class T>
  OutputIterator replace_copy (InputIterator first, InputIterator last,
                               OutputIterator result,
                               const T& old_value, const T& new_value);
```

把`[first, last)`之间的元素拷贝到`result`的过程中，遇到`old_value`则向`result`中写入`new_value`。返回新range的`end`位置。

```C++
template <class InputIterator, class OutputIterator, class UnaryPredicate, class T>
  OutputIterator replace_copy_if (InputIterator first, InputIterator last,
                                  OutputIterator result, UnaryPredicate pred,
                                  const T& new_value);
```

把`[first, last)`之间的元素拷贝到`result`的过程中，遇到元素满足`pred`则向`result`中写入`new_value`。返回新range的`end`位置。

## 填充

* `fill`
* `fill_n`

```C++
template <class ForwardIterator, class T>
  void fill (ForwardIterator first, ForwardIterator last, const T& val);
```

给一个范围填充同一个值。

```C++
template <class OutputIterator, class Size, class T>
  OutputIterator fill_n (OutputIterator first, Size n, const T& val);
```

给从`first`开始的区域填充`n`个`val`。返回新range的`end`位置。

## 生成数据

* `generate`
* `generate_n`

```C++
template <class ForwardIterator, class Generator>
  void generate (ForwardIterator first, ForwardIterator last, Generator gen);
```

从`first`到`last`，调用`gen`生成值并填充进去

```C++
template <class OutputIterator, class Size, class Generator>
  OutputIterator generate_n (OutputIterator first, Size n, Generator gen);
```

从`first`开始，调用`n`次`gen`生成数据并写入。返回新range的`end`位置。

## 删除

* `remove`
* `remove_if`
* `remove_copy`
* `remove_copy_if`

```C++
template <class ForwardIterator, class T>
  ForwardIterator remove (ForwardIterator first, ForwardIterator last, const T& val)
```

删除`[first, last)`中所有的值为`val`的元素。返回新的`end`。判断相等使用`operator==`。该函数并不能改变原始container的属性，比如大小和留下元素的顺序，只能告知新的`end`在什么地方。

```C++
template <class ForwardIterator, class UnaryPredicate>
  ForwardIterator remove_if (ForwardIterator first, ForwardIterator last,
                             UnaryPredicate pred);
```

删除`[first, last)`中所有满足`pred`的元素。返回新的`end`。该函数并不能改变原始containerd的属性，比如大小和留下元素的顺序，只能告知新的`end`在什么地方。

```C++
template <class InputIterator, class OutputIterator, class T>
  OutputIterator remove_copy (InputIterator first, InputIterator last,
                              OutputIterator result, const T& val);
```

将`[first, last)`拷贝到`result`开始的新range过程中，忽略所有值为`val`的元素。比较采用`operator==`。

```C++
template <class InputIterator, class OutputIterator, class UnaryPredicate>
  OutputIterator remove_copy_if (InputIterator first, InputIterator last,
                                 OutputIterator result, UnaryPredicate pred);
```

将`[first, last)`拷贝到`result`开始的新range过程中，忽略所有满足`pred`的元素。新旧range之间不可以有overlap。

## 去重

* `unique`
* `unique_copy`

```C++
template <class ForwardIterator>
  ForwardIterator unique (ForwardIterator first, ForwardIterator last);

template <class ForwardIterator, class BinaryPredicate>
  ForwardIterator unique (ForwardIterator first, ForwardIterator last,
                          BinaryPredicate pred);
```

去掉sequence里的连续重复，对于任何一个连续重复，只保留第一个element。**并不是去掉所有的重复元素**。判断条件可以是`operator==`或者是满足`pred`。返回新的`end`。

```C++
template <class InputIterator, class OutputIterator>
  OutputIterator unique_copy (InputIterator first, InputIterator last,
                              OutputIterator result);

template <class InputIterator, class OutputIterator, class BinaryPredicate>
  OutputIterator unique_copy (InputIterator first, InputIterator last,
                              OutputIterator result, BinaryPredicate pred);
```

将`[first, last)`拷贝到`result`开始的新range过程中，忽略所有连续满足`operator==`的重复元素或者满足`pred`的连续重复元素。新旧range之间不可以有overlap。

## 反转

* `reverse`
* `reverse_copy`

```C++
template <class BidirectionalIterator>
  void reverse (BidirectionalIterator first, BidirectionalIterator last);
```

将`[first, last)`范围内前后位置反转。内部调用了`iter_swap`。

```C++
template <class BidirectionalIterator, class OutputIterator>
  OutputIterator reverse_copy (BidirectionalIterator first,
                               BidirectionalIterator last, OutputIterator result);
```

将`[first, last)`范围拷贝到`result`开始的新位置，但元素顺序反转。

## 平移

* `rotate`
* `rotate_copy`

```C++
template <class ForwardIterator>
  ForwardIterator rotate (ForwardIterator first, ForwardIterator middle,
                          ForwardIterator last);
```

`middle`不是`[first, last)`范围的中位，而是中间某个位置。函数执行之后这个位置将是开始位置，前面的元素放在最后。效果类似汇编语言循环移位操作。返回值是原来的`first`。

```C++
template <class ForwardIterator, class OutputIterator>
  OutputIterator rotate_copy (ForwardIterator first, ForwardIterator middle,
                              ForwardIterator last, OutputIterator result);
```

类似`rotate`，但实现的循环移位是在以`result`为开始的新range。

## 洗牌

* `shuffle`
* `random_shuffle`

```C++
template <class RandomAccessIterator, class URNG>
  void shuffle (RandomAccessIterator first, RandomAccessIterator last, URNG&& g);
```

使用URNG(Uniform Random Number Generator)生成随机位置的shuffle算法。实现见[这里](http://www.cplusplus.com/reference/algorithm/shuffle/)

```C++
template <class RandomAccessIterator>
  void random_shuffle (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class RandomNumberGenerator>
  void random_shuffle (RandomAccessIterator first, RandomAccessIterator last,
                       RandomNumberGenerator&& gen);
```

第一个是使用内建的随机数发生器来产生随机位置，第二个则是有用户指定的随机数发生器来生成随即位置。