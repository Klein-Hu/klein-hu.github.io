---
layout: post
title: The Little Book of Semaphores Reading Notes - Concepts
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes, semaphore]
fullview: false
comments: true
published: false
---

This is the reading notes of Allen B. Downey's *The Little Book of Semaphores*

# Semaphore

A **semaphore** is a special integer:
1. Initialized with an integer; Only could INC/DEC; Cannot check current value
2. If DEC result is negative, the thread which did it will be blocked
3. If INC happened to a semaphore, no matter the value of semaphore is negative or not, one of waiting thread will be activated.

Deeper thinking about the semaphore:
1. Before DEC, the thread does not know it will be blocked or not.
2. After INC, if a thread waked up:
    1. No way to know which thread got waked up
    2. The waked up thread and the thread calling INC will run concurrently.
3. When calling INC, there is no way to know whethere where is another thread is waiting.

# Rendezvous

A **rendezvous** is that two threads rendezvous at a point of execution, and neither is allowed to proceed until both have arrived.

## Solution
**Thread A**
```Python
statement a1
aArrived.signal()
bArrived.wait()
statement a2
```

**Thread B**
```Python
statment b1
bArrived.signal()
aArrived.wait()
statment b2
```

**Note**: If `wait()` before `signal()`, it will cause deadlock.

# Mutex

The **mutex** guarantees that only one thread accesses the shared variable at a time. Other analogies would be:
1. a token that passes from one thread to another, allowing one thread at a time to proceed.
2. a semaphore initialized to 1.

**Definition**
* If both threads have to run the same code, the solution is **symmetric**.
* If the threads have to run different code, the solution is **asymmetric**. 

# Multiplex

No more than n threads can run in the critical section at the same time. This pattern is called **multiplex**.

To implement this multiplex, just initialize the semaphore to `n`.

# Barrier

The synchronization requirement is that no thread executes critical point until after all threads have executed rendezvous.

## Solution

**Definition**
```Python
n = the number of threads
count = 0
mutex = Semaphore(1)
barrier = Semaphore(0)
```

**Thread**
```Python
rendezvous

mutex.wait()
    count = count + 1
mutex.signal()

if count == n:
    barrier.signal()

barrier.wait()
barrier.signal()

critical point
```

This pattern, a wait and a signal in rapid succession, occurs often enough that it has a name; it’s called a **turnstile**, because it allows one thread to pass at a time, and it can be locked to bar all threads.

# Reusable Barrier

**Definition**
```Python
turnstile = Semaphore(0)
turnstile2 = Semaphore(1)
mutex = Semaphore(1)
```

**Solution 1**
```Python
rendezvous

mutex.wait()
    count = count + 1
    if count == n:
        turnstile2.wait()
        turnstile.signal()
mutex.signal()

turnstile.wait()
turnstile.signal()

critical point

mutex.wait()
    count = count - 1
    if count == 0:
        turnstile.wait()
        turnstile2.signal()
mutex.signal()

turnstile2.wait()
turnstile2.signal()
```

This solution is sometimes called a **two-phase barrier** because it forces all the threads to wait twice: once for all the threads to arrive and again for all the threads to execute the critical section.

**Solution 2**

We can simplify the solution if the thread that unlocks the turnstile preloads the turnstile with enough signals to let the right number of threads through.

```Python
rendezvous

mutex.wait()
    count = count + 1
    if count == n:
        turnstile.signal()
mutex.signal()

turnstile.wait()

critical point

mutex.wait()
    count = count - 1
    if count == 0:
        turnstile2.signal()
mutex.signal()

turnstile2.wait()
```

**Barrier Object**
```Python
class Barrier:
	def __init__(self, n):
		self.n = n
		self.count = 0
		self.mutex = Semaphore(1)
		self.turnstile = Semaphore(0)
		self.turnstile2 = Semaphore(0)

	def phase1(self):
		self.mutex.wait()
			self.count += 1
			if self.count == self.n:
				self.turnstile.signal(self.n)
		self.mutex.signal()
		self.turnstile.wait()

	def phase2(self):
		self.mutex.wait()
			self.count -= 1
			if self.count == 0:
				self.turnstile2.signal(self.n)
		self.mutex.signal()
		self.turnstile2.wait()

	def wait(self):
		self.phase1()
		self.phase2()
```
Usage
```Python
barrier = Barrier(n)
barrier.wait ()
```

# Queue

In this case, the semaphore initial value is 0, and usually the code is written so that it is not possible to signal unless there is a thread waiting, so the value of the semaphore is never positive.

**Definition**
```Python
leaderQueue = Semaphore(0)
followerQueue = Semaphore(0)
```

**Solution(leaders)**
```Python
followerQueue.signal()
leaderQueue.wait()
dance()
```

**Solution(followers)**
```Python
leaderQueue.signal()
followerQueue.wait()
dance()
```

# Exclusive Queue

**Definition**
```Python
leaders = followers = 0
mutex = Semaphore(1)
leaderQueue = Semaphore(0)
followerQueue = Semaphore(0)
rendezvous = Semaphore(0)
```

**Solution(leaders)**
```Python
mutex.wait()
if followers > 0:
    followers -= 1
    followerQueue.signal()
else:
    leaders += 1
    mutex.signal()
    leaderQueue.wait()
dance()
rendezvous.wait()
mutex.signal()
```

**Solution(followers)**
```Python
mutex.wait()
if leaders > 0:
    leaders -= 1
    leaderQueue.signal()
else:
    followers += 1
    mutex.signal()
    followerQueue.wait()
dance()
rendezvous.signal()
```

**Note**: When a semaphore is used as a queue4, I find it useful to read “wait” as “wait for this queue” and signal as “let someone from this queue go.”

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)
