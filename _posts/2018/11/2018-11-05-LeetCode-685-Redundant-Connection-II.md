---
layout: post
title: Disjoint-set - Redundant Connection II
categories: [Algorithm]
tags: [LeetCode, Disjoint-set, Algorithm]
fullview: false
comments: true
---

Original Problem: [685. Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/description/)

The related question [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/description/) is a traditional disjoint-set problem. But this question is slightly different - it is a **directed** graph. We could not just remove the last edge causing circle. We need to consider the direction, which mean the last edge need to leave but remove another one. A fact is, each of node in a tree must have in-degree 1. If more than 1, one of the in-edge must be the target to remove. 


```C++
class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int N = edges.size();
        vector<int> root(N+1, 0);
        vector<int> first, second;
        
        for (auto& e : edges)
        {
            if (root[e[1]] == 0)
            {
                root[e[1]] = e[0];
            }
            else
            {
                first = { root[e[1]], e[1] };
                second = e;
                e[1] = 0;
            }
        }
        
        for (int i = 0; i <= N; i++) root[i] = i;
        
        for (auto& e : edges)
        {
            if (e[1] == 0) continue;
            
            int x = get_root(root, e[0]);
            int y = get_root(root, e[1]);
            
            if (x == y) return first.empty() ? e : first;
            root[x] = y;
        }
        
        return second;
    }
    
    int get_root(vector<int>& root, int i)
    {
        return i == root[i] ? i : get_root(root, root[i]);
    }
};
```