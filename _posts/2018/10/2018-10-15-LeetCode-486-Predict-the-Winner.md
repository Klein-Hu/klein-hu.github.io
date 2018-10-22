---
layout: post
title: DP - Predict the Winner
categories: [Algorithm]
tags: [LeetCode, DP, Algorithm]
fullview: false
comments: true
---

Original Problem: [486. Predict the Winner](https://leetcode.com/problems/predict-the-winner/description/)

The idea will be: if length is `N`, A chooses one, then problem becomes a `N-1` one. Then it becomes a DP problem. The information we need to keep for each round will be the difference between first choose and second choose. 

```C++
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int N = nums.size();
        vector<vector<int>> dp(N, vector<int>(N, 0));
        
        for (int i = 0; i < N; i++)
        {
            dp[i][i] = nums[i];
        }
        
        for (int len = 1; len < N; len++)
        {
            for (int i = 0, j = len; j < N; i++, j++)
            {
                dp[i][j] = max(nums[i] - dp[i+1][j], nums[j] - dp[i][j-1]);
            }
        }
        
        return dp[0][N-1] >= 0;
    }
};
```