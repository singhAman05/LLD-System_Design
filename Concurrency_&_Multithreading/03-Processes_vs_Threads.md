---
title: "Processes vs Threads"
series: "Concurrency & Multithreading"
order: 3
tags: [concurrency, processes, threads, os, memory]
---

# Processes vs Threads

## Overview

Both processes and threads are units of execution, but they differ fundamentally in how they share resources and communicate.

| Aspect | Process | Thread |
|--------|---------|--------|
| Memory | Own address space (isolated) | Shared address space (within process) |
| Creation cost | Heavy (OS allocates new memory) | Light (shares parent's memory) |
| Communication | IPC: pipes, sockets, shared memory | Direct memory access (shared heap) |
| Crash impact | One process crashes → others survive | One thread crashes → entire process dies |
| Context switch | Expensive (full memory map swap) | Cheaper (same address space) |

---

## What is a Process?

A **process** is an independent program in execution. It has its own:

- Address space (code, data, heap, stack)
- File descriptors
- Security context
- Environment variables

```
Process A                    Process B
┌──────────────────┐        ┌──────────────────┐
│  Code segment     │        │  Code segment     │
│  Data segment     │        │  Data segment     │
│  Heap             │        │  Heap             │
│  Stack            │        │  Stack            │
│  File descriptors │        │  File descriptors │
└──────────────────┘        └──────────────────┘
     ISOLATED                     ISOLATED
```

### Key Properties

- **Isolation:** Process A cannot read Process B's memory (enforced by OS + hardware)
- **Independence:** If Process A crashes, Process B continues running
- **Heavy creation:** The OS must allocate a full virtual memory space

### When to Use Processes

- Running untrusted code (isolation for security)
- Services that must survive independently (microservices)
- CPU-intensive computation where crash isolation matters
- Multi-language systems (each process can run different runtime)

---

## What is a Thread?

A **thread** is a lightweight unit of execution within a process. Multiple threads share the same address space.

```
Process
┌─────────────────────────────────────┐
│  Code segment (shared)               │
│  Data segment (shared)               │
│  Heap (shared)                       │
│                                      │
│  ┌─────────┐ ┌─────────┐ ┌────────┐ │
│  │ Thread 1 │ │ Thread 2 │ │Thread 3│ │
│  │  Stack   │ │  Stack   │ │ Stack  │ │
│  │  PC      │ │  PC      │ │ PC     │ │
│  │  Regs    │ │  Regs    │ │ Regs   │ │
│  └─────────┘ └─────────┘ └────────┘ │
└─────────────────────────────────────┘
```

### What Threads Share

- Code segment
- Data segment (global variables)
- Heap memory
- File descriptors
- Signal handlers

### What Each Thread Has Independently

- Program counter (PC) — where it is in execution
- Register set — CPU state
- Stack — local variables, function call frames
- Thread-local storage (TLS)

### When to Use Threads

- Tasks that need shared data (avoids IPC overhead)
- Responsiveness (UI thread + worker threads)
- Server handling (thread per client or thread pool)
- Tasks within a single logical application

---

## Communication

### Inter-Process Communication (IPC)

Since processes can't access each other's memory, they need explicit mechanisms:

| Mechanism | Speed | Complexity | Use Case |
|-----------|-------|-----------|----------|
| Pipes | Fast | Low | Parent-child data flow |
| Message queues | Medium | Medium | Decoupled producers/consumers |
| Shared memory | Fastest | High | High-throughput data sharing |
| Sockets | Slow | Medium | Network/cross-machine |

### Inter-Thread Communication

Threads share memory directly — no IPC needed. But this creates race conditions:

```cpp
// Shared variable
int counter = 0;

// Thread 1                    // Thread 2
counter++;                     counter++;
// Both read 0, both write 1 — counter ends up as 1, not 2
```

**Solution:** Synchronization primitives (mutexes, semaphores, atomic operations).

---

## Performance Comparison

### Creation Time

```
Process creation:  ~10-100ms (fork + exec on Linux)
Thread creation:   ~0.01-0.1ms (pthread_create)
```

Threads are 100-1000x cheaper to create.

### Memory Overhead

```
New process:  Full virtual address space (~few MB minimum)
New thread:   Just a stack (~1-8 MB) + register set
```

### Context Switch Cost

```
Process switch: ~1-10 μs (TLB flush, page table swap)
Thread switch:  ~0.1-1 μs (same address space, no TLB flush)
```

---

## Real-World Examples

### Web Browser (Both)

```
Browser Process Architecture:
├── Main process (UI, tabs management)
├── Renderer process 1 (Tab: Google)    ← Process isolation
├── Renderer process 2 (Tab: YouTube)   ← Crash in one tab won't kill others
└── GPU process

Within each renderer process:
├── Main thread (DOM, layout)
├── Compositor thread (painting)        ← Threads for performance
└── Worker threads (JavaScript)
```

Chrome uses **processes** for tab isolation (security + stability) and **threads** within each process for performance.

### Web Server

```
Apache (process-based):
├── Master process
├── Worker process 1 → handles client A
├── Worker process 2 → handles client B
└── Worker process 3 → handles client C

Nginx (thread-based event loop):
├── Master process
└── Worker process
    ├── Event loop (handles 1000s of connections)
    └── Thread pool (for blocking I/O)
```

---

## Decision Matrix

| Factor | Choose Processes | Choose Threads |
|--------|-----------------|----------------|
| Need crash isolation? | ✅ Yes | ❌ No |
| Need shared memory? | ❌ Expensive IPC | ✅ Direct access |
| Creating many instances? | ❌ Heavy | ✅ Lightweight |
| Security between tasks? | ✅ OS-enforced isolation | ❌ Any thread can access any data |
| Different languages? | ✅ Each has own runtime | ❌ Must be same language |
| Simple communication? | ❌ Needs IPC setup | ✅ Shared variables |

---

## Takeaway

> **Processes** give you isolation and safety — use them when tasks must not interfere with each other. **Threads** give you efficiency and easy communication — use them when tasks are cooperating within the same application. Most real systems use both: processes for fault boundaries, threads for performance within each boundary.
