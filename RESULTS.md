# Assignment 5 – Results

## Overview
This assignment implements a user-level threading library with cooperative scheduling and mutex synchronization in xv6.

## Context Switching
Threads are implemented in user space by maintaining a separate stack and saved register context for each thread. Context switching is performed using `uswtch.S`, which saves the current thread’s registers and restores the next thread’s registers. This effectively switches execution between threads by swapping stack pointers.

## Scheduler
A round-robin cooperative scheduler is used. Threads must explicitly call `thread_yield()` to allow other threads to run. Only one thread runs at a time.

## Thread Management
- Threads are stored in a fixed-size table (maximum 16 threads)
- Each thread has its own stack allocated with `malloc`
- Threads transition through states: RUNNABLE, RUNNING, and ZOMBIE
- `thread_join()` waits for a thread to complete and frees its stack

## Mutex Implementation
A simple cooperative mutex is implemented:
- `mutex_lock()` waits while the lock is held and calls `thread_yield()`
- `mutex_unlock()` releases the lock
No atomic operations are required because scheduling is cooperative.

## Demonstration Program
The `test_pc` program implements a producer-consumer problem:
- Two producer threads add items to a shared buffer
- One consumer thread removes items
- A mutex protects access to the buffer

### Output
The program produces output such as:
consumer got 100 items ...
consumer got 200 items ...
consumer got 300 items ...
consumer got 400 items ...
test_pc: done

This demonstrates:
- Correct synchronization (no data corruption)
- No deadlock (program completes)
- Proper scheduling of threads

## Limitations
- Maximum of 16 threads
- Fixed stack size (4096 bytes per thread)
- No preemptive scheduling (threads must yield manually)
- No priority-based scheduling
