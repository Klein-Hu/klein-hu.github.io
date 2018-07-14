---
layout: post
title: The Little Book of Semaphores Reading Notes - Classic Synchronization Problems - 01 - Dining Philosophers
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes, semaphore]
fullview: false
comments: true
published: true
---

This is the reading notes of Allen B. Downey's *The Little Book of Semaphores*.

## Traditional Problem

A table with five plates, five forks (or chopsticks) and a big bowl of spaghetti. Five philosophers, who represent interacting threads, come to the table and execute the following loop: 1) Think, 2) Get forks, 3) Eat, 4) Put forks. The thing that makes the problem interesting, unrealistic, and unsanitary, is that the philosophers need two forks to eat, so a hungry philosopher might have to wait for a neighbor to put down a fork.

It is easy to figure out:

* Only one philosopher can hold a fork at a time.
* It must be impossible for a deadlock to occur.
* It must be impossible for a philosopher to starve waiting for a fork.
* It must be possible for more than one philosopher to eat at the same time.

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)
