---
layout: post
title: Sliding Puzzle
categories: [Algorithm]
tags: [LeetCode, BFS]
fullview: false
comments: true
---

Original Problem: [773. Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/)

From the question it is about moving puzzle and seems like keeping current state in stack and try next step. But this question does not need to know the actual solution but only how many steps. Then we could treat each move as a node in a graph, and the edge is the move. This is not a complex graph, because for each node, there are at most 3 outgoing edges. Then we could travel the graph from original node with **BFS**. For each round, the move count increase 1, until we find the goal. 

```C++
class Solution {
private:
    string to_string(vector<vector<int>>& p)
    {
        string s;
        for (int i = 0; i < p.size(); ++i) {
            for (int j = 0; j < p[0].size(); ++j) {
                s += p[i][j] + '0';
            }
        }
        return s;
    }

    bool try_next_step(queue<vector<vector<int>>>& q, vector<vector<int>>& p, unordered_set<string>& s, int i, int j, int x, int y)
    {
        swap(p[x][y], p[i][j]);
        string layout = to_string(p);
        if (layout == "123450") return true;

        if (s.find(layout) == s.end()) 
        {
            s.insert(layout);
            q.push(p);
        }

        swap(p[x][y], p[i][j]);
        
        return false;
    }
    
    bool add_next_step(queue<vector<vector<int>>>& q, vector<vector<int>>& p, unordered_set<string>& s)
    {
        int i = 0;
        int j = 0;
        bool found0 = false;
        for (i = 0; i < 2; i++)
        {
            for (j = 0; j < 3; j++)
            {
                if (p[i][j] == 0)
                {
                    found0 = true;
                    break;
                }
            }

            if (found0) break;
        }
        
        bool found = false;
        if (i-1 > -1)
        {
            found = try_next_step(q, p, s, i, j, i-1, j);
            if (found) return true;
        }
        
        if (i+1 < 2)
        {
            found = try_next_step(q, p, s, i, j, i+1, j);
            if (found) return true;
        }
        
        if (j-1 > -1)
        {
            found = try_next_step(q, p, s, i, j, i, j-1);
            if (found) return true;
        }
        
        if (j+1 < 3)
        {
            found = try_next_step(q, p, s, i, j, i, j+1);
            if (found) return true;
        }
        
        return false;
    }
public:
    int slidingPuzzle(vector<vector<int>>& board) {
        string layout = to_string(board);
        if (layout == "123450") return 0;
        
        queue<vector<vector<int>>> q1;
        queue<vector<vector<int>>> q2;
        unordered_set<string> s;
        
        q1.push(board);
        int result = 1;
        bool found = false;
        while(!q1.empty())
        {
            auto p = q1.front();
            q1.pop();
            bool done = add_next_step(q2, p, s);
            
            if (done) return result;
            
            if (q1.empty())
            {
                q1 = q2;
                queue<vector<vector<int>>> temp;
                swap(q2, temp);
                
                result++;
            }
        }
        
        return -1;
    }
};
```