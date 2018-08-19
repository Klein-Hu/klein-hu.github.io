---
layout: post
title: DP - Distinct Subsequences
categories: [Algorithm]
tags: [LeetCode, Algorithm, DP]
fullview: false
comments: true
---

Original problem: [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/description/)

This is an obvious DP problem: ask for count/max/min.

I know it will be TLE if I list all possibilities. But just give it a shot for fun!

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        int M = s.length();
        int N = t.length();
        if (N > M) return 0;
        
        int result = 0;
        int pos[N];
        memset(&pos, -1, N * sizeof(int));
        
        for (int x = 0; x < M; x++)
        {
            if (s[x] == t[0]) 
            {
                pos[0] = x;
                break;
            }
        }
        
        int i = 0;
        int j = 0;
        while (j >= 0)
        {
            if (j == N)
            {
                result++;

                j--;
                i = pos[j] + 1;
                pos[j] = -1;
            }

            for (; i < M; i++)
            {
                if (s[i] == t[j])
                {
                    pos[j] = i;
                    break;
                }
            }
            
            if (pos[j] == -1)
            {
                j--;
                if (j > -1)
                {
                    i = pos[j] + 1;
                    pos[j] = -1;
                }
            }
            else
            {
                j++;
                i++;
            }
        }
        
        return result;
    }
};
```

Idel solution is to find the DP formula: similar to [Edit Distance](https://leetcode.com/problems/edit-distance/description/): `dp[i][j]` means how many matched subsequence from `S[j-1]` to `T[i-1]`

* if `S[j-1]` == `T[i-1]`, we will have `dp[i][j] = dp[i-1][j-1] + dp[i][j-1]`
    * match `S[j-1]` and `T[i-1]`, then successful matches before them will be `S[0 --> j-2]` to `T[0 --> i-2]` ==> `dp[i-1][j-1]`
    * not match them, then then successful matches before them will be `S[0 --> j-2]` to `T[0 --> i-1]` ==> `dp[i][j-1]`
* if `S[j-1]` != `T[i-1]`, then we have to give up matching them, then the successful matches count will be `dp[i][j-1]`

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        int M = s.length();
        int N = t.length();
        if (N > M) return 0;
        
        int dp[N+1][M+1];
        memset(dp, 0, (N+1) * (M+1) * sizeof(int));
        
        for (int i = 0; i <= M; i++) dp[0][i] = 1;
        for (int i = 1; i <= N; i++) dp[i][0] = 0;
        
        for (int i = 1; i <= N; i++)
        {
            for (int j = 1; j <= M; j++)
            {
                dp[i][j] = dp[i][j-1] + (s[j-1] == t[i-1] ? dp[i-1][j-1] : 0);
            }
        }
        
        return dp[N][M];
    }
};
```