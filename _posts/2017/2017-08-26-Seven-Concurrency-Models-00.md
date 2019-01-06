---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 00
categories: [Reading]
tags: [Design, Design-pattern, Concurrency, Parallel]
fullview: false
comments: true
---

This is the first reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks*. It is an introduction and describes the scope of the book.

### Concurrency and Parallelism

According to [Rob Pike](http://concur.rspace.googlecode.com/hg/talk/concur.html):

* __Concurrency__ is about __dealing with__ lots of things at once.
* __Parallelisam__ is about __doing__ lots of things at once.

My understanding:

* __Concurrency__ is about one CPU core running multiple threads, no matter they are related or not.
* __Parallelism__ is about _x_ CPU cores running exactly _x_ threads and there is no relationship between these threads.

### Parallel Architecture

#### Bit-Level
* Comparing 8-bit and 32-bit

#### Instruction-Level
* Pipeline
* Out-of-order execution
* Speculative execution

#### Data-Level 
* SIMD: Single Instruction, Multiple Data
* Same operation (e.g. increase brightness of image) on a large quantity of data in parallel

#### Task-Level
* Shared Memory, using memory directly
* Distributed Memory, using network

### Seven Models

#### Threads and Locks
* The basic idea of most of other models

#### Functional Programming
* Get rid of mutable state.

#### The Clojure Way - Separating Identity and State
* Hybrid of imperative and functional programming

#### Actors
* Based on message passing
* Good for both shared- and distributed- memory architecture

#### Communicating Sequential Processes
* Based on message passing
* Emphasis on channels used for communication

#### Data Parallelism
* Using the power of GPU

#### The Lambda Architecture
* Combination of MapReduce and stream processing

### Quesion We Should Ask for Each Models
* What is the model shooting for? Concurrent problems or parallel problems or both?
* Which parallel architecture(s) can this model target?
* Does this model have right tool for resilent or geo distributed code?

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)