---
title: "What is Concurrency?"
series: "Concurrency & Multithreading"
order: 1
tags: [concurrency, multithreading, os, systems]
---

# What is Concurrency?

## Definition

**Concurrency** is the ability of a system to handle multiple tasks that are _in progress_ at the same time. It does NOT mean they execute simultaneously — it means their execution can overlap in time.

> Concurrency is about **dealing with** many things at once. Parallelism is about **doing** many things at once.
> — Rob Pike

---

## The Restaurant Analogy

Think of a single chef (one CPU core) in a kitchen:

- **Sequential:** Cook dish A completely → then cook dish B completely
- **Concurrent:** Start boiling water for A → while waiting, chop vegetables for B → check on A → plate B → finish A

The chef isn't doing two things at the same instant. But by interleaving tasks intelligently, both dishes are ready faster than doing them one after another.

---

## Why Concurrency Matters

| Problem | Without Concurrency | With Concurrency |
|---------|--------------------|-----------------| 
| UI responsiveness | App freezes during file download | Download runs in background, UI stays responsive |
| Server handling | One client at a time, others wait | Multiple clients handled concurrently |
| Resource utilization | CPU idle during I/O waits | CPU does useful work during I/O waits |

---

## Key Concepts

### 1. Task Interleaving

A single processor switches between tasks rapidly, giving the _illusion_ of simultaneous execution.

```
Time →
Task A: ████░░░░████░░░░████
Task B: ░░░░████░░░░████░░░░

████ = Running    ░░░░ = Waiting
```

The OS scheduler decides when to switch between tasks.

### 2. Context Switching

When the CPU switches from one task to another, it must:

1. Save the current task's state (registers, program counter, stack pointer)
2. Load the next task's state
3. Resume execution of the next task

This has a cost — context switches are not free.

### 3. Non-Determinism

Concurrent programs can produce different results on different runs because the order of task switching is controlled by the OS, not the programmer.

```cpp
// Thread 1
counter++;  // Read → Increment → Write

// Thread 2
counter++;  // Read → Increment → Write

// If counter starts at 0, result could be 1 OR 2
// depending on interleaving
```

This is the root of most concurrency bugs.

---

## Concurrency vs Sequential Execution

```
Sequential:
┌─────────┐ ┌─────────┐ ┌─────────┐
│  Task A  │ │  Task B  │ │  Task C  │
└─────────┘ └─────────┘ └─────────┘
Total time: A + B + C

Concurrent (single core):
┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐┌──┐
│A ││B ││C ││A ││B ││C ││A ││B ││C │
└──┘└──┘└──┘└──┘└──┘└──┘└──┘└──┘└──┘
Total time: Similar to A + B + C, but better responsiveness
```

**Key insight:** Concurrency doesn't always make things faster on a single core. It makes systems more **responsive** and better at utilizing **idle time** (like I/O waits).

---

## When to Use Concurrency

| Use Case | Why |
|----------|-----|
| I/O-bound tasks | CPU is idle waiting for disk/network — use that time |
| User interfaces | Keep UI responsive while doing background work |
| Server applications | Handle many clients without blocking on each one |
| Real-time systems | Meet timing requirements by managing multiple activities |

---

## Challenges of Concurrency

| Challenge | Description |
|-----------|-------------|
| Race conditions | Multiple tasks accessing shared data with unpredictable ordering |
| Deadlocks | Tasks waiting for each other indefinitely |
| Starvation | A task never gets CPU time because others dominate |
| Debugging | Non-deterministic bugs that don't reproduce consistently |

---

## Takeaway

> Concurrency is a design approach — structuring your program so multiple tasks can make progress without necessarily running at the same physical instant. It's essential for responsive, efficient, real-world software.
