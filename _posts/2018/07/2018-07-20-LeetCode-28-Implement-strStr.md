---
layout: post
title: String Matching - Sunday Algorithm
categories: [Algorithm]
tags: [LeetCode, String]
fullview: false
comments: true
---

[LeetCode 28](https://leetcode.com/problems/implement-strstr/description/) is an "Easy" question. But I learned a new algorithm, which is cool!

## Typical Solutions

Usually, the first solution on top of my mind is KMP. It is quite popular in most of traditional algorithm books. If dig more, BM is the next one, which is in some of case could improve the efficiency, because it could jump further.

## From KMP to BM

Comparing KMP and BM, the helper array generation is similar, only difference is KMP is left->right while BM is right to left. The improvement in BM is when there is characters in `s` and `t` not match, KMP usually jumping LESS than BM. In this case, BM is faster than KMP.

## From BM to Sunday

Sunday algorithm seems only improved a little from BM: when characters in `s` and `t` not match, check the next character right after current window in  `s`. If it is not in `t`, directly jump the whole compare window; if it is in `t`, jump to the first occurrence from right. Also, Sunday does not like BM and KMP which restrict the comparison must right->left or left->right. It could be any arbitrary order in Sunday algorithm. The helper arry is also easy to build: for each char, the last position in `t` is what you want.

For performance, it is easy to see the worst case will be `O(n)`, which mean everytime the mismatch happening at the last comparison in each window. But in the most optimistic case, the performance could be `O(n/m)`.

## The Code

```C++
class Solution {
public:
    int strStr(string haystack, string needle) {
        if (needle.length() == 0) return 0;
        if (needle.length() > haystack.length()) return -1;
        
        int M = haystack.length();
        int N = needle.length();
        
        int pos[256] = { -1 };        
        for (int i = 0; i < N; i++)
        {
            pos[needle[i]] = i;
        }
        
        int i = 0; 
        int j = 0;
        
        int sh = 0;        
        while (i < M && sh + N <= M)
        {
            if (haystack[i] == needle[j])
            {
                i++;
                j++;
                
                if (j == needle.length())
                {
                    return sh;
                }
            }
            else
            {
                if (pos[haystack[sh + N]] != -1)
                {
                    int p = pos[haystack[sh + N]]; 
                    sh = sh + N - p;
                    i = sh;
                    j = 0;
                }
                else
                {
                    sh += N + 1;
                    i = sh;
                    j = 0;
                }
            }
        }
        
        return -1;
    }
};
```

## Reference

1. [A Very Fast Substring Search Algorithm](https://csclub.uwaterloo.ca/~pbarfuss/p132-sunday.pdf)
2. [Sunday Algorithm](http://www.inf.fh-flensburg.de/lang/algorithmen/pattern/sundayen.htm)
3. [Exact String Matching Algorithm](http://www-igm.univ-mlv.fr/~lecroq/string/)