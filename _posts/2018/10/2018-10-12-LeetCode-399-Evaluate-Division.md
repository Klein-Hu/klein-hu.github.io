---
layout: post
title: Graph - Evaluate Division
categories: [Algorithm]
tags: [LeetCode, Graph, Algorithm]
fullview: false
comments: true
---

Original Problem: [399. Evaluate Division](https://leetcode.com/problems/evaluate-division/description/)

Last time when I tried this question, I used DFS. This time I decide to try something new: The Floyd for shortest path in graph. The good part of this problem is: no matter DFS or Floyd, you could easily figure out the solution, but to make it actually working for you, it is not easy.

```C++
class Solution {
public:
    vector<double> calcEquation(vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries) {
        vector<double> result;
        if (queries.empty()) return result;
        
        int N = equations.size();
        unordered_map<string, unordered_map<string, double>> m;
        for (int i = 0; i < N; i++)
        {
            string x = equations[i].first;
            string y = equations[i].second;
            
            if (m.find(x) == m.end()) m[x] = unordered_map<string, double>();
            if (m.find(y) == m.end()) m[y] = unordered_map<string, double>();
            m[x][x] = 1.0;
            m[y][y] = 1.0;
            m[x][y] = values[i];
            m[y][x] = 1 / values[i];
        }
        
        for (auto t : m)
        {
            string k = t.first;
            for (auto row : m)
            {
                string x = row.first;
                for (auto col : m)
                {
                    string y = col.first;
                    if (m[x].find(k) != m[x].end() && m[k].find(y) != m[k].end())
                    {
                        m[x][y] = m[x][k] * m[k][y];
                    }
                }
            }
        }
        
        for (auto p : queries)
        {
            if (m.find(p.first) != m.end() && m[p.first].find(p.second) != m[p.first].end()) result.push_back(m[p.first][p.second]);
            else result.push_back(-1);
        }
        
        return result;
    }
};
```