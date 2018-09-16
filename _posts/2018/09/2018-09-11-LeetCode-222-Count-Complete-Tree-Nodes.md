---
layout: post
title: Tree - Count Complete Tree Nodes
categories: [Algorithm]
tags: [LeetCode, Tree, Recursive, Algorithm]
fullview: false
comments: true
---

Original Problem: [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

Interesting question because of its setup: BFS will TLE on test case 15. Improve it will stop on test case 16. This is the amazing part I like LeetCode more than LintCode: the accuracy of performance of my algorithm. I could understand some variation of test result, but sometimes double the run time in LintCode is not quite ideal to understand whether my answer is good or not.

So, finally tried the recursive solution: if left-left path and right-right path height are equal, calculate the nodes; if not, separately count both sides and put numbers together.

**Lesson Learned**: Recursive solution may not the slowest solution, it depends on how the recursive ends. If it could stop recursive calls earlier, then it is better than reach nodes one by one.

```C++
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;

        TreeNode* p = root;
        TreeNode* q = root;
        int llevels = 0;
        int rlevels = 0;
        while (p != nullptr)
        {
            p = p->left;
            llevels++;
        }

        while (q != nullptr)
        {
            q = q->right;
            rlevels++;
        }

        if (llevels == rlevels) return (1 << llevels) - 1;

        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```
