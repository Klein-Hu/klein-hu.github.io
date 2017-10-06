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

Basic knowledge and concepts. But one puzzle is interesting:

> Assume the `count` is a global variable and `temp` is a local one. If below loop is the thread body of 100 threads, what is the smallest possible value of `count` after all threads have completed.

> ```
> for i in range(100):
>     temp = count
>     count = temp + 1
> ```

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)
