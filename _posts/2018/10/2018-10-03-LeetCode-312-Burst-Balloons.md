---
layout: post
title: DP - Burst Balloons
categories: [Algorithm]
tags: [LeetCode, Heap, Algorithm]
fullview: false
comments: true
---

Original Problem: [312. Burst Balloons](https://leetcode.com/problems/burst-balloons/description/)

A special DP question: aparently the length `n` change make difference here. But what is the `result(n) = f(result(n-1))` is the key. Let's try from 1 balloon, 2 balloons, and more. Then we could figure out 

    `result(k) = nums(-1) * nums(k) * nums(n) + result(1 -> k-1) + result(k+1 -> n-1)`

Then, there are 3 levels of loop;

1. length: 1 -> N
2. range: from M to N - length + 1
3. In the range: k from begin() to the end(), caculate each `dp[begin][end]`, comparing with existing value, keep the max one.

Finally, the dp[1][N] will be the result.
 
```C++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int N = nums.size();
        vector<vector<int>> dp(N+2, vector<int>(N+2, 0));        
        
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        for (int len = 1; len <= N; len++)
        {
            for (int b = 1; b <= N-len+1; b++)
            {
                int e = b + len - 1;
                for (int k = b; k <= e; k++)
                {
                    dp[b][e] = max(dp[b][e], nums[b-1]*nums[k]*nums[e+1] + dp[b][k-1] + dp[k+1][e]);
                }
            }
        }
        
        return dp[1][N];
    }
};
```