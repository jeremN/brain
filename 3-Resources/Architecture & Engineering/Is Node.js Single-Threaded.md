---
url: https://nodesource.com/blog/is-nodejs-single-threaded-or-not
category: "Architecture & Engineering"
tags:
  - nodejs
  - threading
  - concurrency
  - architecture
date_saved: "2026-03-11"
---

# Is Node.js Single-Threaded?

**Source:** [https://nodesource.com/blog/is-nodejs-single-threaded-or-not](https://nodesource.com/blog/is-nodejs-single-threaded-or-not)
**Category:** Architecture & Engineering

## Summary

NodeSource's deep dive into Worker Threads in Node.js. Explains why Node.js is single-threaded (one process, one thread, one event loop) and when that becomes a problem: CPU-intensive operations block the event loop. Worker Threads solve this by allowing multiple threads within a single process, each with its own event loop and JS engine instance. Key mechanisms: SharedArrayBuffer for shared memory between threads, MessagePort/MessageChannel for communication, Atomics for concurrent operations, and WorkerData for startup data.

## Why It's Interesting

Clear mental model for when to use Workers vs async I/O — essential for Node.js performance optimization decisions

## Key Takeaways

- Node.js runs on a single thread — only I/O operations run in parallel (async), CPU-intensive work blocks the event loop
- Worker Threads provide: one process, multiple threads, one event loop per thread, one JS engine instance per thread
- SharedArrayBuffer allows sharing memory between threads (limited to binary data), avoiding costly JSON serialization
- MessagePort and MessageChannel enable structured data communication between parent and worker threads
- Use Worker pools in production rather than spawning new Workers per task — the overhead of creating Workers isn't free
- Workers are for CPU-intensive work, NOT for I/O parallelism — async I/O is already more efficient than Workers for I/O

## Tags

#nodejs, #threading, #concurrency, #architecture
