---
layout: post
title: Greedy - Task Scheduler
categories: [Algorithm]
tags: [LeetCode, Greedy, Algorithm]
fullview: false
comments: true
---

Original Problem: [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/description/)

First ide came to my mind was short task should be done earlier. But apparently it is not the right thing for this problem because the task could be run in any order at any time and don't have to execute once for all. The only thing impact overal latency is the cool-down time.

Then the idea was count the tasks' count, sort the count. Let's say `m` is the last task's count. Then the `(m-1) * (n+1)` should be filled, with tasks or idles. Then just try to fill all tasks in, if any spaces not filled, then it could be idles. Count all tasks and idles, the total number would be the result.

```C++
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        if (tasks.empty()) return 0;
        
        int count[26] = {0};
        for (char c : tasks) count[c - 'A']++;
        
        sort(count, count+26);
        
        int x = -1;
        for(int j = 0; j < 26; j++)
        {
            if (count[j] != 0)
            {
                x = j;
                break;
            }
        }
        
        int i = 25;
        for (; i > x; i--)
        {
            if (count[i] > count[i-1]) break;
        }
        
        return max((int)tasks.size(), (count[25]-1) * (n+1) + 25 - i + 1);
    }
};
```