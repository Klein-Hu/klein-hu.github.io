---
layout: post
title: Seven Concurrency Models in Seven Weeks Reading Notes - 06
categories: [tech]
tags: [design-pattern, concurrency, parallel, thread, lock, read-notes]
fullview: false
comments: true
published: false
---

This is the reading notes of Paul Butcher's *Seven Concurrency Models in Seven Weeks* - "Data Parallelism". The sample code is in C and [OpenCL](https://www.khronos.org/opencl/).

Basic ways to implement data parallelism in GPU: pipelining and multiple ALUs.

Programming with OpenCL, as a developer, your work is to divide the problem into small **work-items**. The OpenCL compiler and runtime will take care the scheduling part. In the majority of cases, you should aim for maximum parallelism and the smallest possible work-items and worry about optimization only afterward.

We specify how each work-item should be processed by writing an OpenCL **kernel**.

An OpenCL **context** represents an environment within which OpenCL kernels can execute. OpenCL code, aka *kernel*, is read as string and executed in the C code. The data which kernel is working on is in the **buffer**. Result data needs to be retrieved from output buffer. Finally, the environment need to be cleaned up. OpenCL also provide profiling tool to check the performance. The error handling of OpenCL is similar to Windows HRESULT.

A real-world OpenCL application would either perform more involved operations on its operands or work on data that was already resident on the GPU.

Data parallelism is ideal whenever you’re faced with a problem where large amounts of numerical data needs to be processed. The current toolset is very much focused on number-crunching.

Optimizing an OpenCL kernel can be tricky, and effective optimization often depends on understanding underlying architectural details. This can be a particular issue if you need to write high-performance cross-platform code.

### Open Readings
1. 

---
![E=mc^2]({{ site.url }}/assets/images/emc2.gif)