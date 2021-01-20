---
title: 'Operating Systems: An Intro to Concurrency and Scheduling'
date: '01-19-2021'
---

Have you ever wondered how your computer runs so many applications simultaneously? Take Google Chrome, for example. It runs each tab in its own isolated process. So let's say your computer has 1 CPU with 2 physical cores. At any given moment, this level of hardware supports the __execution__ of two programs. Now I don't know about you, but I have way more than 2 Chrome tabs open at a time. But Google Chrome aside, there's a good chance your PC is running hundreds of processes. Commodity PCs don't include hundreds of CPUs, so how is this possible? We'll explore this question in detail, and understand how the OS gives off the illusion of the simultaneous execution of multiple processes.

First, it's always helpful to define a few basic terms that will come up a bunch:

- **Concurrency**: A quick Google search on concurrency in a computing context will yield the following: 

    *"the ability to execute more than one program or task simultaneously"*.
    
    I find this a bit misleading however. Our computers are definitely capable of running multiple applications at the same time, but they cannot all be utilizing the CPU(s) at the same time. So, a better definition might look like:
    
    *"the actual or apparent ability to execute more than one program or task simultaneously"*.

- **Physical Core** - The basic unit of execution inside a CPU. Each physical core, dismissing hyperthreading, is capable of executing one program at any given moment in time.

TODO
