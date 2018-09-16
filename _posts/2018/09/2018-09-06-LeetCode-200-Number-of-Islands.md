---
layout: post
title: Flags - Number of Islands
categories: [Algorithm]
tags: [LeetCode, BFS, Algorithm]
fullview: false
comments: true
---

Original Problem: [200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

Not a fancy question and no fancy solution: mark where has been checked, go through the whole grid.

Two details:

1. Whether we should mark the spot before put neighbours in the queue or do it when pick up from the queue.
2. Whether we should have a `vector<vector<bool>>` for flag or just directly update the grid.

```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.empty() || grid[0].empty()) return 0;

        int result = 0;
        for (int i = 0; i < grid.size(); i++)
        {
            for (int j = 0; j < grid[0].size(); j++)
            {
                if (grid[i][j] == '1')
                {
                    result++;

                    queue<pair<int, int>> q;
                    pair<int, int> tmp = make_pair(i, j);
                    q.push(tmp);
                    grid[i][j] = '2';

                    while(!q.empty())
                    {
                        auto p = q.front();
                        q.pop();

                        int x = p.first;
                        int y = p.second;

                        if (x-1 >= 0 && grid[x-1][y] == '1') { grid[x-1][y] = '2'; q.push(make_pair(x-1, y)); }
                        if (y-1 >= 0 && grid[x][y-1] == '1') { grid[x][y-1] = '2'; q.push(make_pair(x, y-1)); }
                        if (x+1 < grid.size() && grid[x+1][y] == '1') { grid[x+1][y] = '2'; q.push(make_pair(x+1, y)); }
                        if (y+1 < grid[0].size() && grid[x][y+1] == '1') { grid[x][y+1] = '2'; q.push(make_pair(x, y+1)); }
                    }
                }
            }
        }

        return result;
    }
};
```