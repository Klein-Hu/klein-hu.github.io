---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 01
categories: [Reading]
tags: [Design, Design-pattern, Concurrency, Parallel]
fullview: false
comments: true
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - Threads and Locks. The sample code in this chapter is JAVA. `java.util.concurrent` is the package. But we should not use `Thread` class in the PROD code.

### Mutual Exclusion and Memory Models

Usually, memory models are more complex than mutual exclusion and locks (race condidtion, dead lock, etc.).

Three common issues of multi-thread programs:
* Race condition
* Memory visibility
* Dead lock

Your environment sometimes could introduce more randomization by:
* Compiler static optimization could reorder things in one thread and causing another thread output looks unexpected.
* JVM (or other VM the code running on) could optimize the code and reorder things.
* The hardware could reorder things as well.

Deadlock could happen between current working thread and call-back method/function (a.k.a. __alien method__), because we don't know if the callback method having any lock in it or not. To solve this situation, simply make a copy of all callback functions/listeners, exit current lock, then  iterating them. Now even there is a callback function which will hold anther lock, it is still good, because current lock in working thread has been unlocked. It will also improve the performance.

### Beyond Intrinsic Locks

Limitation of Intrinsic locks:
1. No way to interrupt the wait
2. No way to timeout
3. Lock require-release has to be in the same method and has to be nested.

**ReentrantLock** is introduced to solve the limitation of instrinsic lock.
1. `ReentrantLock`-`lockInterruptibly()` together. Calling a thread's `interrupt()` method will interrupt the lock. 
2. `ReentrantLock`-`tryLock(int, TimeUnit)` together provide the timeout for lock.

Hand-over-Hand locking is a solution of a set/list of object, which only lock a small potion and allow other threads access the rest of set/list.

**Condition Variable** is designed to be a flag of something happened. To use it, the thread needs to be in a critical session and use while loop to check and `await()` the condition variable. Once it is true, then the thread could access shared resource. After access resource, signal others about this condition variable.

**Atomic Variable** is something like `AtomicInteger` in Java:
1. Atomic variable is the only way to implement lock-free algorithm.
2. Never forget any lock operations.
3. Never causing deadlock.

`volatile` is a keyword in Java, which only make sure the read/write of a variable will not be reordered. It is no relationship to lock or concurrency.

### Important data structures and utilities in `java.util.concurrent`

Creating thread for each of incoming Internet request will end up DoS attack. **Thread Pool** is the right solution for it.The size of the thread pool usually is # of cpu_core, if tasks are computation-sensitive; some larger number is perfered if tasks are IO-sensitive.

`CopyOnWriteArrayList` in Java is a special data structure in `java.util.concurrent`. It makes a copy whenever the list is modified.

`ArrayBlockingQueue` is a queue for producer-consumer question. The `put()` and `take()` will block if the queue is full and empty. 
`ConcurrentLinkedQueue` is an unbounded, wait-free and nonblocking queue.

In Java, `HashMap` is not thread-safe. So every access to this kind of object need to have lock-unlock operation. `ConcurrentHashMap` is the thread-safe one. The technology used here is **Lock Striping**.

### Different ways to solve "Dining Philosophers"

1. Give each chopsticks an Id. Always get two chopsticks in a fixed global order. It is ok for simple problem like this, but for large project, it is quite hard to do so.
2. When every time try to get the chopstick, use `tryLock` which will avoid deadlock. But the performance of this solution is really bad.
3. Each philosopher has a condition variable. When one philosopher is going to be in "thinking" state, it signals left and right ones after set itself "eating" state to false. When he tries to eat, he checks wether left and right are both not in "eating" state. If left or right one is in "eating" state, current one is `await()` his condition. The "lock" of this solution is the "table", which is more efficient than any other solutions.

### Misc

What makes multithreaded programming difficult is not that writing it is hard, but that **testing it is hard**. Pretty much **logging** is the only way to understand the failure when it happens and can't reproduce locally.

### Open Readings

1. [Java Memory Model](http://docs.oracle.com/javase/specs/jls/se7/html/jls-17.html#jls-17.4)
2. William Pugh, "Java memory model" website
3. JSR 133 (Java memory model), especially the "FAQ" session.
4. ReentrantLock supports a fairness parameter. What does it mean for a lock to be “fair”?
5. What is `ReentrantReadWriteLock`? How does it differ from `ReentrantLock`?
6. What is a “spurious wakeup”?
7. What is `AtomicIntegerFieldUpdater`? How does it differ from `AtomicInteger`?
8. What is **work-stealing**?
9. What is the difference between a **CountDownLatch** and a **CyclicBarrier**?
10. What is **Amdahl’s law**?

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)