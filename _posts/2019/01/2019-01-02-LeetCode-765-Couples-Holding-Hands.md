---
layout: post
title: Couples Holding Hands
categories: [Algorithm]
tags: [LeetCode, Algorithm]
fullview: false
comments: true
---

Original Problem: [765. Couples Holding Hands](https://leetcode.com/problems/couples-holding-hands/)

This is a good problem: easy to understand, but no easy solution at beginning. 

At least, it could be a greedy alogrithm: for each pair, if they are not a couple, from the rest of the list, find the couple of the first in the pair, swap them found element with the second in the pair; then move on to the next pair. 

A trick is: if `x` and `y` are couple, then `x ^ 1 == y` is true. Or we could use the even and odd number to check the couple.

Here comes the code:

```C++
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int result = 0;
        int N = row.size();
        
        for (int i = 0; i < N; i+=2)
        {
            if ((row[i] % 2 != 0 && row[i] == row[i+1]+1) 
                || (row[i] % 2 == 0 && row[i]+1 == row[i+1])) continue;
            
            int target = (row[i] % 2 == 0) ? row[i]+1 : row[i]-1;
            for (int j = i + 1; j < N; j++)
            {
                if (row[j] == target) 
                {
                    swap(row[i+1], row[j]);
                    result++;
                    break;
                }
            }
        }
        
        return result;
    }
};
```

There is a [Union Find](https://en.wikipedia.org/wiki/Disjoint-set_data_structure) solution from [Grandy Yang's blog](http://www.cnblogs.com/grandyang/p/8716597.html). The most interesting part is the `find()` method. This is an elegant way to implement union joint.

(Below code is from http://www.cnblogs.com/grandyang/p/8716597.html)
```C++
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int res = 0, n = row.size(), cnt = n / 2;
        vector<int> root(n, 0);
        for (int i = 0; i < n; ++i) root[i] = i;
        for (int i = 0; i < n; i += 2) {
            int x = find(root, row[i] / 2);
            int y = find(root, row[i + 1] / 2);
            if (x != y) {
                root[x] = y;
                --cnt;
            }
        }
        return n / 2 - cnt;
    }
    int find(vector<int>& root, int i) {
        return (i == root[i]) ? i : find(root, root[i]);
    }
};
```