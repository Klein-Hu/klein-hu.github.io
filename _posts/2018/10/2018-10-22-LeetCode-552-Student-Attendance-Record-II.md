---
layout: post
title: DP - Student Attendance Record II
categories: [Algorithm]
tags: [LeetCode, DP, Algorithm]
fullview: false
comments: true
---

Original Problem: [552. Student Attendance Record II](https://leetcode.com/problems/student-attendance-record-ii/description/)

The problem is a clear DP problem. The most interesting part is the formula of the DP. 

The requirements:

* No more than one `A` in the result
* No more than two continuous `L`

This means for the No. `i` char in the result:

* If `result[i]` is `P`, then the `0 -> i-1` could be any combinations. ==> `dp[i-1]`
* If `result[i]` is `A`, then the `0 -> i-1` could only be `P` and `L`. And there will be no more than 2 `L` in a row. ==> `dp_pl[i-1]`
* If `result[i]` is `L`:

  * If `result[i-1]` is `P`, then `0 -> i-2` could be any combinations. ==> `dp[i-2]`
  * If `result[i-1]` is `A`, then `0 -> i-2` could be only `P` and `L`. And there will be no more than 2 `L` in a row. ==> `dp_pl[i-2]`
  * If `result[i-1]` is `L`:

    * If `result[i-2]` is `P`, then `0 -> i-3` could be any combination. ==> `dp[i-3]`
    * If `result[i-2]` is `A`, then `0 -> i-3`could be only `P` and `L`. And there will be no more than 2 `L` in a row. ==> `dp_pl[i-3]`

Then the sub-problem is how to calculate the array of `dp_pl` for only `P` and `L` result. It is similar:

* If `result_pl[i]` is `P`, then the `0 -> i-1` could be any combination. ==> `dp_pl[i-1]`
* If `result_pl[i]` is `L`:

  * If `result_pl[i-1]` is `P`, then `0 -> i-2` could be any combination. ==> `dp_pl[i-2]`
  * If `result_pl[i-1]` is `L`, then the `result_pl[i-2]` could only be `P` and `0 -> i-3` could be any combination. ==> `dp_pl[i-3]`

So, the solution is like below:

```C++
class Solution {
public:
    int checkRecord(int n) {
        int m = 1000000007;
        if (n == 0) return 1;
        if (n == 1) return 3;
        if (n == 2) return 8;
        
        
        vector<unsigned long long> dppl(n+1, 0);
        dppl[0] = 1;
        dppl[1] = 2;
        dppl[2] = 4;
        for (int i = 3; i <= n; i++)
        {
            dppl[i] = (dppl[i-1] + dppl[i-2] + dppl[i-3]) % m;
        }
        
        
        vector<unsigned long long> dp(n+1, 0);
        
        dp[0] = 1;
        dp[1] = 3;
        dp[2] = 8;
        
        for (int i = 3; i <= n; i++)
        {
            dp[i] = (dp[i-1] + dppl[i-1] + dp[i-2] + dp[i-3] + dppl[i-2] + dppl[i-3]) % m;
        }
        
        return dp[n];
    }
};
```
