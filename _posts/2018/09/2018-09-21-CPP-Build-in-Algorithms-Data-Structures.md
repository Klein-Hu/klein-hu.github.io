---
layout: post
title: C++ Build-in Algorithms and Data Structures
categories: [Reading]
tags: [Career, C++, Languages]
fullview: false
comments: true
---

刷题中发现C++其实有很多build-in的算法和数据结构是非常好用的，比如[LeetCode 295](https://leetcode.com/problems/find-median-from-data-stream/)，直接用`priority_queue`就能AC，直接用`vector` + `make_heap()`会TLE。于是决定静下心了读文档，整理一份笔记，有关C++ build-in的数据结构和算法。

本文来自于[cplusplus.com reference](http://www.cplusplus.com/reference/algorithm/)

# <algorithm>

## 只读操作

