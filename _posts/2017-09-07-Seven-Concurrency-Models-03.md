---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 03
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: false
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - "The Clojure Way — Separating Identity from State". The sample code is in [Clojure](http://clojure.org).

I'm not an expert of Clojure nor functional programming. So only understand part of the story this time.

Clojure’s atoms are built on top of `java.util.concurrent.atomic`
Clojure’s data structures are **persistent**

Persistent data structures behave as though a complete copy is made each time they’re modified. --- This is why it is slow and memory footprint big.

**Persistent data structures separate identity from state.**

A variable in an imperative language complects (interweaves, interconnects) identity and state.

The way to implement atomic is **retries** - It will discard the value returned by the function and call it again with the atom’s new value. 
To support **atom**, we could also have **validator** (validation *before* value changed) and **watcher**(a function which is called when value changed). The only thing we need to pay attention is by the time the watch function is called, the value of the atom may already have changed again, so watch functions should always use the values passed as arguments and never dereference the atom.

### Open Readings
1. [Understanding Clojure’s PersistentVector Implementation](http://blog.higher-order.net/2009/02/01/understanding-clojures-persistentvector-implementation){:target="_blank"}
2. The implementation of PersistentHashMap using a[Hash Array Mapped Trie](https://en.wikipedia.org/wiki/Hash_array_mapped_trie){:target="_blank"}

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)