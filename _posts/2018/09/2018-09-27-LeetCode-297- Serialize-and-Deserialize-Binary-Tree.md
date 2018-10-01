---
layout: post
title: Tree - Serialize and Deserialize Binary Tree
categories: [Algorithm]
tags: [LeetCode, Heap, Algorithm]
fullview: false
comments: true
---

Original Problem: [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)

Second time try this question. AC directly (after fix some typoes): 

* Serialization: BFS, `*` for nullptr;
* Deserialization: other than the root, each time read 2 numbers/nullptrs. Build by levels.

```C++
class Codec {
private:
    TreeNode* get_node(string& data, int& ind)
    {
        int j = ind;
        while (j < data.length() && data[j] != ' ') j++;
        
        string t = data.substr(ind, j - ind);
        TreeNode *p = nullptr;
        if (t[0] != '*')
        {
            int x = stoi(t);
            p = new TreeNode(x);
        }
        
        ind = j < data.length() ? j+1 : j;
        return p;
    }
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        stringstream ss;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty())
        {
            TreeNode* p = q.front();
            q.pop();
            
            if (p == nullptr)
            {
                ss << "*" << " ";
            }
            else
            {
                ss << p->val << " ";
                q.push(p->left);
                q.push(p->right);
            }
        }
        
        return ss.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int i = 0;

        TreeNode* root = this->get_node(data, i);
        queue<TreeNode*> qq;
        qq.push(root);
        
        while(i < data.length() && !qq.empty())
        {
            TreeNode* p = this->get_node(data, i);
            TreeNode* q = this->get_node(data, i);            
            
            TreeNode* t = qq.front();
            qq.pop();
                        
            t->left = p;
            t->right = q;
            
            if (p != nullptr) qq.push(p);
            if (q != nullptr) qq.push(q);
        }
        
        return root;
    }
};
```