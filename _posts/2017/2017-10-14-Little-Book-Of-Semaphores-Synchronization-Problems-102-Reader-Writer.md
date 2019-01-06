---
layout: post
title: The Little Book of Semaphores Reading Notes - Classic Synchronization Problems - 02 - Reader-Writer
categories: [Reading]
tags: [Design, Design-pattern, Concurrency, Parallel, Semaphore, Synchronization, Synchronization]
fullview: false
comments: true
published: true
---

This is the reading notes of Allen B. Downey's *The Little Book of Semaphores*.

## Traditional Problem

### Requirement
1. Any number of readers could read the data at the same time.
2. Only one writer and no reader could write the data.

The situation is called **categorical mutual exclusion**.
Below solution is good to solve the problem, but it may cause the writer starve.

### Initialization
```Python
int readers = 0
mutex = Semaphore(1)
roomEmpty = Semaphore(1)
```

### Writer
```Python
roomEmpty.wait()
    # Critical section for writers
roomEmpty.signal()
```

### Reader
```Python
mutex.wait()
    readers += 1
    if readers == 1:
        roomEmpty.wait()
mutex.signal()

# critical section for readers

mutex.wait()
    readers -= 1
    if readers == 0:
        roomEmpty.signal()
mutex.signal()
````

## Lightswitch 
Most important data structure we could use to solve Reader-Writer problem

```Python
class Lightswitch:
    def __init__(self):
        self.counter = 0
        self.mutex = Semaphore(1)
    
    def lock(self, semaphore):
        self.mutex.wait()
            self.counter += 1
            if self.counter == 1:
                semaphore.wait()
        self.mutex.signal()

    def unlock(self, semaphore):
        self.mutex.wait()
            self.count -= 1
            if self.count == 0:
                semaphore.signal()
        self.mutex.signal()
```

## No Writer Starve Solution

### Requirement
1. All requirements for traditional Reader-Write problem.
2. When a writer arrives, the existing readers can finish, but no additional readers may enter.

### Initialization
```Python
readSwitch = Lightswitch()
roomEmpty = Semaphore(1)
turnstile = Semaphore(1)
```

### Writer
```Python
turnstile.wait()
    roomEmpty.wait()
    # critical section for writers
turnstile.signal()
roomEmpty.signal()
```

### Reader
```Python
turnstile.wait()
turnstile.signal()
readSwitch.lock(roomEmpty)
    # critical section for readers
readSwitch.unlock(roomEmpty)
```

**Note**: Thus, this solution guarantees that at least one writer gets to proceed, but it is still possible for a reader to enter while there are writers queued.

## Writer-priority Solution

### Requirement
1. All requirements for no writer stave solution.
2. Once a writer arrives, no readers should be allowed to enter until all writers have left the system.

### Initialization
```Python
readSwitch = Lightswitch()
writeSwitch = Lightswitch()
noReaders = Semaphore(1)
noWriters = Semaphore(1)
```

### Reader
```Python
noReaders.wait()
    readSwitch.lock(noWriters)
noReaders.signal()
# critical section for readers
readSwitch.unlock(noWriters)
```

### Writer
```Python
writeSwitch.lock(noReaders)
    noWriters.wait()
        # critical section for writers
    noWriters.signal()
writeSwitch.unlock(noReaders)
```

**Note**: A drawback of this solution is that now it is possible for readers to starve (or at least face long delays).

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)
