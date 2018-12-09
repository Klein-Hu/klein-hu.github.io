---
layout: post
title: My Calendar II
categories: [Algorithm]
tags: [LeetCode, Algorithm]
fullview: false
comments: true
---

Original Problem: [731. My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

Nothing special in the question, brute force should be good enough to solve the problem.

```C++
class MyCalendarTwo {
private:
    vector<pair<int, int>> booked;
    vector<pair<int, int>> dbooked;
public:
    MyCalendarTwo() {

    }

    bool book(int start, int end) {
        for (auto p : dbooked)
        {
            if (isOverlap(p.first, p.second, start, end)) return false;
        }

        for (auto p : booked)
        {
            if (isOverlap(p.first, p.second, start, end))
            {
                dbooked.push_back(getOverlap(p.first, p.second, start, end));
            }
        }

        booked.push_back(make_pair(start, end));

        return true;
    }

private:
    bool isOverlap(int left, int right, int start, int end)
    {
        return left < end && right > start;
    }

    pair<int, int> getOverlap(int estart, int eend, int start, int end)
    {
        return make_pair(max(estart, start), min(eend, end));
    }
};
```