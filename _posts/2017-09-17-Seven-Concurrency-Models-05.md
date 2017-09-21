---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 05
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: false
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - "Communicating Sequential Processes". The sample code is in [Clojure](http://clojure.org).

**CSP**: Communicating Sequential Processes. It is experiencing a renaissance due to GoLang. Two pillars of CSP are *channels* and *go blocks*.

In this chapter, the focus in on the infrastructure of communication, channels, go blocks, etc.

A **channel** is a thread-safe queue—any task with a reference to a channel can add messages to one end, and any task with a reference to it can remove messages from the other. By default it is unbuffered, which means writing to a channel blocks until something read from it. But we could create buffered channel to make the write operation complete immediately if there is still spaces in the buffer.

Buffer Types:
  - **Blocking Buffer**: when buffer is full, new insertion will be blocked.
  - **Dropping Buffer**: when buffer is full, new insertion will not be blocked but not silently dropped.
  - **Sliding Buffer**: when buffer is full, new insertion will not be blocked but replace existing oldest entry.

**Thread Pool** is good for CPU-insentive tasks, which means the thread will be used in a short time and returned back to the pool for reuse. But if threads need communicate with each other, threads in the pool will eventually be empty because all of them may waiting for something. In this case, the **Event-driven** programming will be the way to go, which is like the UI programming solution. But this will also introduce complexity of the mix of state and concurrency. Finally solution: **Go Blocks**. **go** is the macro in Clojure and Lisps. When threads from pool running the macro, it will break in to several pieces and each time execute one piece and then update the state and wait for the next thread pick it up to execute the next piece.


### Open Readings
1. [Core Async Go Macro Internals - Part I](https://www.youtube.com/watch?v=R3PZMIwXN_g)
2. [Core Async Go Macro Internals - Part II](https://www.youtube.com/watch?v=SI7qtuuahhU)
3. [The State Machines of core.async](http://hueypetersen.com/posts/2013/08/02/the-state-machines-of-core-async/)


---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)