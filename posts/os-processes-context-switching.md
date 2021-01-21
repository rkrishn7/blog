---
title: 'Operating Systems: An Intro to Concurrency and Context Switching'
date: '01-19-2021'
---

Have you ever wondered how your computer runs so many applications simultaneously? Google Chrome, for example, runs each tab as its own process. So if you have ten tabs open, that's ten processes your computer has to run. Chrome aside, most computers are running hundreds if not thousands of processes. Commodity PCs don't include hundreds of CPUs, so how is this possible? We'll explore this question in detail, and understand how the Operating System is capable of running so many processes.

First, let's define a few basic terms that will come up a bunch:

- **Concurrency**: A quick Google search on concurrency in a computing context will yield the following: 

    *"the ability to execute more than one program or task simultaneously"*.
    
    I find this a bit misleading however. Our computers are definitely capable of running multiple applications at the same time, but they cannot all be utilizing the CPU(s) at the same time. So, a better definition might look like:
    
    *"the actual or apparent ability to execute more than one program or task simultaneously"*.

- **Physical Core** - The basic unit of execution inside a CPU. Each physical core is capable of executing one program at any given moment in time.

- **Context Switching** - The OS's ability to switch programs on and off the CPU.

The Operating System, at its core, is a resource manager. It's responsible for managing and scheduling the execution of multiple user processes and threads. It would be too risky to delegate this responsibility to user processes. One might get greedy and never yield the CPU. In that case, all of the other programs would be waiting indefinitely. This touches on a very important point: for a program to be *doing* anything, it needs to be on the CPU. If a running program isn't on the CPU, it's either waiting for an I/O response, sleeping, or waiting for some resource. So, one of the OS's primary responsibility is manage which processes are running on the CPU(s). Here's a diagram that shows the multiple states a process can be in:

![Process State Diagram](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gate2009.png)

When a process is created, it moves to the `Ready` queue. It is the Operating System's responsiblity to manage the ready queue and transition programs on and back off the CPU. If a user process makes a blocking I/O request, it moves into the `Blocked` state as it's unable to do anything until it receives the data it needs. When it has received a response, the process is placed back on the `Ready` queue. Also notice how the process can move directly from the `Running` state back to the `Ready` state. Maybe the program is doing something CPU intensive, so in order to not let one program hog all of the available resources, the OS assigns a time slice to all processes. That is, if they've been running for a certain amount of time, they're kicked off the CPU. Time slicing is extremely important, because when a user process isn't running, control is handed back to the OS. Thus, if a user process was able to run at 100% CPU utilization for as long as it wanted, the OS would never be able to regain control. 

