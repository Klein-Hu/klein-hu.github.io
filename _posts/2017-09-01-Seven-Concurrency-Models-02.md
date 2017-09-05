---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 02
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: false
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - Functional Programming. The sample code is in [Clojure](http://clojure.org).

**Imperative Program**: consists of a series of statments that change global state when executed.
**Functional Program**: models computation as the evaluation of expressions, which are first-class and side-effect-free.

Concurrent programming in imperative languages is difficult because of the prevalence of shared mutable state/variable. Clojure is the functional programming eliminating the mutable state/variable. 
* Apply a function to every element of a sequence with `map` or `mapcat`
* Use laziness to handle large, or even infinite, sequences
* Reduce a sequence to a single (possibly complex) value with `reduce`
* Parallelize a reduce operation with `fold`


---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)