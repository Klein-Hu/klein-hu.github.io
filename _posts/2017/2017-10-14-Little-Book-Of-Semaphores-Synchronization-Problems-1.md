---
layout: post
title: The Little Book of Semaphores Reading Notes - Classic Synchronization Problems
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes, semaphore]
fullview: false
comments: true
published: true
---

This is the reading notes of Allen B. Downey's *The Little Book of Semaphores*.

# Producer-Consumer Problem

<mark>Any time you wait for a semaphore while holding a mutex, there is a danger of deadlock.</mark>

## Infinite Buffer

### Ideas
1. Consumer needs to monitor item counts in the buffer. 
2. No limitation on producer.
3. Need lock/mutex on buffer access

### Producer
```Python
event = waitForEvent()
mutex.wait()
    buffer.add(event)
    items.signal()
mutex.signal()
```

### Consumer
```Python
items.wait()
mutex.wait()
    event = buffer.get()
mutex.signal()
event.process()
```

**Note**: In `Consumer` code, if put `items.wait()` into the `mutex` block, it will cause deadlock when there is no items in the buffer.

## Finite Buffer

### Ideas
1. Consumer needs to monitor item counts in the buffer. 
2. Producer needs to monitor the empty slot in the buffer.
3. Need lock/mutex on buffer access

### Producer
```Python
event = createEvent()
spaces.wait()
mutex.wait()
    buffere.add(event)
mutex.signal()
items.signal()
```

### Consumer
```Python
items.wait()
mutex.wait()
    event = buffer.get()
mutex.signal()
spaces.singal()
event.process()
```

**Note**: In order to avoid deadlock, producers and consumers check availability before getting the mutex. For best performance, they release the mutex before signaling.

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)
