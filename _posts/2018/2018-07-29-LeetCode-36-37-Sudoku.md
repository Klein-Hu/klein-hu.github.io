---
layout: post
title: Sudoku? Special Case of 8-Queens
categories: [Algorithm]
tags: [LeetCode, Matrix, Algorithm, 8-Queens]
fullview: false
comments: true
---

Original Problems:[LeetCode 36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/description/) and [LeetCode 37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)

Validation is easy, should be a 10-15 min task. For the sub-box validation, it could be a caculation based on top-left and 3x3 validation, or a hard-code each center cell of each sub-box and then evalute cells around it. Apparently, the hard-code solution is better on performance.

I actually tried to read some articles about how a humanbeing solve the Sudoku. But quickly figured out that for man to check the whole board and figure out the less tries to archieve. But it is not quite right for a program which should finish in 30 min. Maybe there is an AI solution but could not fit in the time. Then it is degenerated to a special case of 8-queens problem, just the rule of validation is different. Recursive solution was used but the non-recursive one is similar as 8-queens solution.

**Validate Sudoku**
```C++
class Solution {
private:
    bool isRowsValid(vector<vector<char>>& board)
    {
        vector<bool> v(9, false);
        for (int i = 0; i < 9; i++)
        {
            v = vector<bool>(9, false);
            for (int j = 0; j < 9; j++)
            {
                int c = board[i][j] - '0';
                if (0 <= c && c <= 9)
                {
                    if (v[board[i][j]]) return false;
                    v[board[i][j]] = true;
                }
            }
        }
                  
        return true;
    }
    
    bool isColumnsValid(vector<vector<char>>& board)
    {
        vector<bool> v(9, false);
        for (int j = 0; j < 9; j++)
        {
            v = vector<bool>(9, false);
            for (int i = 0; i < 9; i++)
            {
                int c = board[i][j] - '0';
                if (0 <= c && c <= 9)
                {
                    if (v[board[i][j]]) return false;
                    v[board[i][j]] = true;
                }
            }
        }
                  
        return true;    
    }
    
    bool isSubBoxesValid(vector<vector<char>>& board)
    {
        for (int i = 0; i < 3; i++)
        {
            for (int j = 0 ; j < 3; j++)
            {
                int x = i * 3;
                int y = j * 3;
                vector<bool> v(9, false);
                for (int r = x; r < x+3; r++)
                {
                    for (int t = y; t < y+3; t++)
                    {
                        int c = board[r][t] - '0';
                        if (0 <= c && c <= 9)
                        {
                            if (v[board[r][t]]) return false;
                            v[board[r][t]] = true;
                        }
                    }
                }
            }
        }
                                  
        return true;
    }
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        return isRowsValid(board) && isColumnsValid(board) && isSubBoxesValid(board);
    }
};
```

**Sudoku Solver**
```C++
class Solution {
private:
    bool isRowValid(vector<vector<char>>& board, int i)
    {
        vector<bool> v = vector<bool>(9, false);
        for (int j = 0; j < 9; j++)
        {
            int c = board[i][j] - '0';
            if (0 <= c && c <= 9)
            {
                if (v[board[i][j]]) return false;
                v[board[i][j]] = true;
            }
        }
        
        return true;
    }
    
    bool isColumnValid(vector<vector<char>>& board, int j)
    {
        vector<bool> v = vector<bool>(9, false);
        for (int i = 0; i < 9; i++)
        {
            int c = board[i][j] - '0';
            if (0 <= c && c <= 9)
            {
                if (v[board[i][j]]) return false;
                v[board[i][j]] = true;
            }
        }
        
        return true;        
    }
    
    bool isSubBoxValid(vector<vector<char>>& board, int x, int y)
    {
        vector<bool> v(9, false);
        for (int r = x; r < x+3; r++)
        {
            for (int t = y; t < y+3; t++)
            {
                int c = board[r][t] - '0';
                if (0 <= c && c <= 9)
                {
                    if (v[board[r][t]]) return false;
                    v[board[r][t]] = true;
                }
            }
        }
        
        return true;
    }
    
    bool dfs(vector<vector<char>>& board, int i, int j)
    {
        if (i == 9) return true;
        if (j == 9) return dfs(board, i+1, 0);
        
        if (board[i][j] != '.')
        {
            return dfs(board, i, j+1);
        }
        else
        {
            for (int k = 1; k <= 9; k++)
            {
                board[i][j] = k + '0';
                if (isRowValid(board, i) && isColumnValid(board, j) && isSubBoxValid(board, (i/3)*3, (j/3)*3))
                {
                    if (dfs(board, i, j+1)) return true;
                }
                
                board[i][j] = '.';
            }
            
            return false;
        }
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        dfs(board, 0, 0);
    }
};
```