---
title: 'Operating Systems: An Intro to Concurrency and Context Switching'
date: '01-19-2021'
---

Have you ever wondered how your computer runs so many applications simultaneously? Most computers are running hundreds if not thousands of processes. Commodity PCs don't include hundreds of CPUs, so how the heck is this possible? To answer this, we'll need to understand what *concurrency* is and how the OS manages multiple program contexts and lets each one get its fair share of time on the CPU. Well, what is *concurrency*?

A quick Google search will return the following definition:

> the ability to execute more than one program or task simultaneously

I find this a bit misleading, however. First, we need to distinguish a program that is *executing* from one that is *running*. A process that is executing is one that is on the CPU. That is, its program code is currently being processed by the CPU. A running process may not be executing. Rather, it might be waiting for some event to occur, sleeping, or in a queue of processes waiting to get back on the processor. So a better definition may be:

> the **actual or apparent** ability to execute more than one program or task simultaneously

Even though we might have **M** running programs, we can only have **N** executing programs, where N is bounded by the available physical resources. For example, if we have 20 running programs and our hardware consists of a dual core CPU, then (dismissing [hyper-threading](https://en.wikipedia.org/wiki/Hyper-threading)) we can only execute 2 programs at any given moment. One of the Operating System's primary responsibility is managing and scheduling the execution of multiple user processes and threads, to give the end user the illusion that all of their programs are executing. Delegating this task to user-level code would pose sever security and, quite possibly, performance risks. One of the running processes may decide it wants to run forever and never yield the CPU. Programs have multiple states they may be in, and the OS must manage the states of all processes and terminate, yield, or interrupt them as necessary.

![Process State Diagram](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gate2009.png)

When a process is created, it moves to the `Ready` queue. The OS must manage the ready queue and schedule programs to move to the `Running` state (on the CPU). If a user process makes a blocking I/O request, it moves into the `Blocked` state as it's unable to do anything until it receives the data it needs. When it has received a response, the process is placed back on the `Ready` queue. A process can also move directly from the `Running` state back to the `Ready` state. In order to not let one program hog all of the available resources, the OS assigns a time slice to all processes. That is, if they've been running for a certain amount of time, they're kicked off the CPU. Time slicing is crucial to keeping the OS in power, because when a user process isn't executing, the OS is. Thus, if a user process was able to run at 100% CPU utilization for as long as it wanted, the OS would never be able to regain control. 
