---
layout: post
title: Wildcard Matching
categories: [Algorithm]
tags: [LeetCode, String, Algorithm]
fullview: false
comments: true
---

Original problem: [44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/description/)

This is my heart broken problem, always, never could AC in one shot in last 20 years ......

There are two solutions for this problem: DP or backtracking. There are so many posts on the Internet about DP but what I like is the backtracking, which is more like what we do the matching manually:

1. From beginning, compare each characters
2. Loop until we checked all characters in `s`:

    1. If current character in `s` and `p`, or it is '?' in `p`, move on.
    2. If it is '*' in `p`, remember current position in both `s` and `p`, then only move on to the next character in `p`.
    3. If current character in `s` and `p` not match:

        1. Check if we have any record from step 3. If not, we failed.
        2. If there is a record from step3, take them as current position (the backtrack part), move the index for `s` to next character since the historical position not working eventually, let's try to match the next one. In the meanwhile, index for `p` also need to move to the next one.

3. Now if we also reach the end of `p`, then perfect, we found a match.
4. If not, we have some leftover in `p`, it is still OK if all leftovers are '*'. Otherwise, we failed.

So simple, but always make mistakes, including typo, when write the code. Shame on me.......