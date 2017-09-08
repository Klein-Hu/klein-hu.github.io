---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 02
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: true
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - Functional Programming. The sample code is in [Clojure](http://clojure.org).

I'm not an expert of Clojure nor functional programming. So only understand part of the story this time.

**Imperative Program**: consists of a series of statments that change global state when executed.
**Functional Program**: models computation as the evaluation of expressions, which are first-class and side-effect-free.

Concurrent programming in imperative languages is difficult because of the prevalence of shared mutable state/variable. Clojure is the functional programming eliminating the mutable state/variable. 
* Apply a function to every element of a sequence with `map` or `mapcat`
* Use laziness to handle large, or even infinite, sequences
* Reduce a sequence to a single (possibly complex) value with `reduce`
* Parallelize a reduce operation with `fold`

`map` is the data transformation. `reduce` is the data consumption. The transformation and consumption is user defined functions.

`map` should be easily parallelized. But `reduce` may not. `fold` can't work with a lazy sequence, because there is no way to perform binary chop on it.

**Dataflow programming**: Whenever the data associated with a function’s inputs becomes available, that function is executed. And each function could (theoretically, at least) execute concurrently. This style of execution is called dataflow programming.

**Referentially transparent**: anywhere an invocation of the function appears, we can replace it with its result without changing the behavior of the program. This is very important to functional programming. Clojure is impure functional programming language and its function could have side effect.

The primary benefit of functional programming is **confidence**, confidence that your program does what you think it does. But functional code will be less efficient than its imperative equivalent.

### Open Readings
1. [Java lambda expressions](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html){:target="_blank"}
2. [Java streams](http://docs.oracle.com/javase/tutorial/collections/streams/index.html){:target="_blank"}

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)