---
layout: post
title: The Little Book of Semaphores Reading Notes - 01
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes, semaphore]
fullview: false
comments: true
published: false
---

This is the reading notes of Allen B. Downey's *The Little Book of Semaphores* - "Introduction". The sample code is in Python-like language.

Basic knowledge and concepts. But two puzzles are interesting:

#### Puzzle 1
Assume the `count` is a global variable and `temp` is a local one. 

```
for i in range(100):
    temp = count
    count = temp + 1
```
**Question**: If below loop is the thread body of 100 threads, what is the smallest possible value of `count` after all threads have completed?

#### Puzzle 2
You and Bob operate a nuclear reactor that you monitor from remote stations. Most of the time, both of you are watching for
warning lights, but you are both allowed to take a break for lunch. It doesn’t matter who eats lunch first, but it is very important that you don’t eat lunch at the same time, leaving the reactor unwatched!

**Question**: Figure out a system of message passing (phone calls) that enforces these restraints. Assume there are no clocks, and you cannot predict when lunch will start or how long it will last. What is the minimum number of messages that is required?

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)
