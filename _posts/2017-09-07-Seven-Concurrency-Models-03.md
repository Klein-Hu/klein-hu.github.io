---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 03
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: true
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

Three of the mechanisms that Clojure provides to supportshared mutable state. 
  - **Atoms** enable independent, synchronous changes to single values.
  - **Agents** enable independent, asynchronous changes to single values
  - **Refs** enable coordinated, synchronous changes to multiple values

Clojure’s functional nature leads to code with few mutable variables. Typically this means that simple atom-based concurrency is sufficient:
  * STM-based code in which multiple refs are coordinated through transactions can be transformed into an agent-based solution with those refs consolidated into a single compound data structure accessed via an agent.
  * The choice between an STM and an agent-based solution is largely one of style and performance characteristics. 
  * Custom concurrency constructs can make code simpler and clearer

### Open Readings
1. [Understanding Clojure’s PersistentVector Implementation](http://blog.higher-order.net/2009/02/01/understanding-clojures-persistentvector-implementation){:target="_blank"}
2. The implementation of PersistentHashMap using a[Hash Array Mapped Trie](https://en.wikipedia.org/wiki/Hash_array_mapped_trie){:target="_blank"}
3. [Persistent Data Structures and Managed References: Clojure’s Approach to Identity and State](https://www.infoq.com/presentations/Value-Identity-State-Rich-Hickey)
4. [Simple Made Easy](https://www.infoq.com/presentations/Simple-Made-Easy)
5. [The Database as a Value](https://www.infoq.com/presentations/Datomic-Database-Value)
6. [Transactional Memory in GCC](http://gcc.gnu.org/wiki/TransactionalMemory)

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)