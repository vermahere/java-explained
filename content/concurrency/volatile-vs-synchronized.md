+++
title = "volatile vs synchronized in Java: What’s the Real Difference?"
description = "Understand the real difference between volatile and synchronized in Java, including memory visibility, atomicity, and common misconceptions."
draft = false
tags = ["java", "concurrency", "volatile", "synchronized"]
categories = ["Concurrency"]
+++

### Introduction

One of the most common questions in Java concurrency is:

> **What is the difference between `volatile` and `synchronized`?**

Both are used in multi-threaded programs.  
Both deal with shared variables.  
Yet they solve **different problems**.

This article explains:
- What `volatile` actually guarantees
- What `synchronized` actually guarantees
- When `volatile` is enough
- When `synchronized` is unavoidable

---

### The Core Difference

At a high level:

- `volatile` → **visibility**
- `synchronized` → **visibility + atomicity + mutual exclusion**

This single distinction explains most of the confusion.

---

### What problem `volatile` solves

In a multi-threaded program, threads may cache variables locally.
This can cause one thread to see **stale values** written by another thread.

Declaring a variable as `volatile` ensures:
- Writes go directly to main memory
- Reads always see the latest value
- Changes made by one thread are immediately visible to others

---

#### Example: Visibility with `volatile`

```java
class Worker {
    private volatile boolean running = true;

    void stop() {
        running = false;
    }

    void run() {
        while (running) {
            // do work
        }
    }
}
```

Here, volatile ensures that when one thread sets running = false,
other threads immediately see the change.

### What `volatile` does not guarantee

volatile does not provide:

- Atomicity

- Mutual exclusion

- Thread safety for compound actions

#### Example: Broken Code

```java
volatile int count = 0;

void increment() {
    count++; // NOT atomic
}
```

#### Why this fails:

- count++ is a read → modify → write operation

- Multiple threads can interleave between these steps

- volatile guarantees visibility, not atomicity

### What problem `synchronized` solves

`synchronized` provides:

- Mutual exclusion (only one thread executes the block at a time)

- Atomicity of operations inside the block

- Memory visibility guarantees

When a thread exits a synchronized block, all changes are flushed to main memory.

#### Example: Correct Synchronization

```java
class Counter {
    private int count = 0;

    synchronized void increment() {
        count++;
    }
}
```

Here:

- Only one thread can execute increment() at a time

- The increment operation is atomic

- Memory visibility is guaranteed

### Performance Considerations

`volatile` is lightweight and non-blocking

`synchronized` involves locking

However:

- Modern JVMs heavily optimize `synchronized`

- Correctness matters more than micro-optimizations

- Using `volatile` incorrectly leads to subtle bugs

### When volatile Is Appropriate

Use `volatile` when:

- A variable is written by one thread and read by others

- No compound operations are involved

- The variable represents a simple state or flag

Typical examples:

- Stop flags

- Status indicators

- Configuration reload flags

### When synchronized Is Required

Use `synchronized` when:

- Shared state is modified

- Operations must be atomic

- Invariants must be preserved

Examples:

- Counters

- Shared collections

- Multi-step updates