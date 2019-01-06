---
layout: post
title: DP - Maximum Product Subarray
categories: [Algorithm]
tags: [LeetCode, DP]
fullview: false
comments: true
---

Original Problem: [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/description/)

It is easy to tell: DP is the solution. But this is a special DP to me. The DP I get use to is like below:

* 1-dimension: like climb up stairs, `dp[i] = f(dp[x1], dp[x2], ... ,dp[xm])` where `x1, x2, ... , xm < i`
* 2-dimension: like edit distance, `dp[i,j] = f(dp[x1][y1], dp[x2][y2], ... , dp[xm][ym])` where `x1, x2, ... , xm < i` and `y1, y2, ... , ym < j`. 

In above case, the last item in the `dp` array will be the answer.

This is a different one:

* 1-dimension, but two arrays: `maxR[i]` remember the max product of subarray which has `nums[i]` as the last element; `minR` remember the min product of subarray which has `nums[i]` as the last element.
* When a new number `nums[i+1]` involved, `maxR[i+1] = max(maxR[i] * nums[i+1], minR[i] * nums[i+1], nums[i+1])`, `minR[i+1] = min(maxR[i] * nums[i+1], minR[i] * nums[i+1], nums[i+1])`.
* A separate variable remember the overall max and return as result at the end.

The reason to remember both `maxR` and `minR` is because if the next element `nums[i+1]` is opposite in contrast, the max could become min. So we have to keep both.

```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        vector<int> maxn(nums.size());
        vector<int> minn(nums.size());
        
        maxn[0] = minn[0] = nums[0];
        int result = nums[0];
        for (int i = 1; i < nums.size(); i++)
        {
            int a = maxn[i-1] * nums[i];
            int b = minn[i-1] * nums[i];
            maxn[i] = max(a, max(b, nums[i]));
            minn[i] = min(a, min(b, nums[i]));
            result = max(result, maxn[i]);
        }
        
        return result;
    }
};
```