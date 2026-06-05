---
title: "Thread Lifecycle & States"
series: "Concurrency & Multithreading"
order: 4
tags: [concurrency, threads, lifecycle, os, scheduling]
---

# Thread Lifecycle & States

## Overview

A thread doesn't just "run." It moves through well-defined states during its lifetime. Understanding these states is critical for debugging concurrency issues and designing thread-safe systems.

---

## Thread States

```
                    ┌─────────────┐
                    │    NEW      │
                    │  (Created)  │
                    └──────┬──────┘
                           │ start()
                           ▼
                    ┌─────────────┐
              ┌─────│  RUNNABLE   │◄────────────────────┐
              │     │  (Ready)    │                      │
              │     └──────┬──────┘                      │
              │            │ Scheduler picks             │
              │            ▼                             │
              │     ┌─────────────┐                     │
              │     │   RUNNING   │─────────────────────┤
              │     │ (Executing) │    Time slice ends   │
              │     └──┬───┬───┬──┘                     │
              │        │   │   │                        │
              │        │   │   │ I/O, sleep,            │
              │        │   │   │ lock wait              │
              │        │   │   ▼                        │
              │        │   │ ┌─────────────┐            │
              │        │   │ │   BLOCKED/  │────────────┘
              │        │   │ │   WAITING   │  I/O done,
              │        │   │ └─────────────┘  lock acquired,
              │        │   │                  notify()
              │        │   │
              │        │   │ run() completes
              │        │   ▼
              │        │ ┌─────────────┐
              │        │ │ TERMINATED  │
              │        │ │   (Dead)    │
              │        │ └─────────────┘
              │        │
              │        │ Exception thrown
              │        └──────────────────► TERMINATED
              │
              │ Thread destroyed before start
              └──────────────────────────► TERMINATED
```

---

## State Descriptions

### 1. NEW (Created)

The thread object exists but hasn't started execution.

```cpp
std::thread t(myFunction);  // Thread is NEW
// Resources allocated, but no CPU time yet
```

**Transitions from NEW:**
- → RUNNABLE: when `start()` is called (or thread begins in C++)

### 2. RUNNABLE (Ready)

The thread is eligible to run and waiting for the CPU scheduler to give it time.

```
Ready Queue: [Thread A] [Thread B] [Thread C]
                         ↑
                    Scheduler picks next
```

**Key point:** A RUNNABLE thread is NOT necessarily running — it's waiting for its turn on the CPU.

**Transitions from RUNNABLE:**
- → RUNNING: scheduler picks this thread
- → TERMINATED: thread is interrupted/cancelled

### 3. RUNNING (Executing)

The thread is actively executing instructions on a CPU core.

```
Core 1: Thread A is RUNNING
Core 2: Thread C is RUNNING
Ready Queue: [Thread B] [Thread D]  ← These are RUNNABLE, not RUNNING
```

**Transitions from RUNNING:**
- → RUNNABLE: time slice expires (preemption), `yield()`
- → BLOCKED/WAITING: I/O request, `sleep()`, waiting for lock, `wait()`
- → TERMINATED: `run()` completes, unhandled exception

### 4. BLOCKED / WAITING

The thread cannot proceed until some condition is met.

| Sub-state | Trigger | Resumes when |
|-----------|---------|--------------|
| Blocked on I/O | Read from disk/network | I/O operation completes |
| Blocked on lock | `mutex.lock()` | Lock becomes available |
| Sleeping | `sleep(duration)` | Duration elapses |
| Waiting | `condition.wait()` | Another thread calls `notify()` |
| Timed waiting | `wait(timeout)` | Timeout or notification |

```cpp
// Thread enters WAITING state
std::unique_lock<std::mutex> lock(mtx);
cv.wait(lock, [] { return dataReady; });  // Blocked until notify

// Another thread wakes it up
dataReady = true;
cv.notify_one();  // Moves waiting thread → RUNNABLE
```

**Transitions from BLOCKED/WAITING:**
- → RUNNABLE: blocking condition resolved

### 5. TERMINATED (Dead)

The thread has finished execution. It cannot be restarted.

```cpp
void worker() {
    // ... do work ...
    return;  // Thread transitions to TERMINATED
}

t.join();  // Main thread waits for t to reach TERMINATED
```

**How a thread reaches TERMINATED:**
- `run()` method returns normally
- Unhandled exception propagates out of `run()`
- Thread is cancelled (platform-specific)

---

## State Transitions in C++

```cpp
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void worker() {
    // RUNNING: doing computation
    std::cout << "Working..." << std::endl;

    // → BLOCKED: acquiring lock
    std::unique_lock<std::mutex> lock(mtx);

    // → WAITING: waiting for condition
    cv.wait(lock, [] { return ready; });

    // → RUNNING: condition met, resuming
    std::cout << "Resumed!" << std::endl;

    // → TERMINATED: function returns
}

int main() {
    std::thread t(worker);  // NEW → RUNNABLE → RUNNING

    std::this_thread::sleep_for(std::chrono::seconds(1));

    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one();  // Moves worker: WAITING → RUNNABLE

    t.join();  // Wait for TERMINATED
}
```

---

## Scheduling and Preemption

The OS scheduler controls when threads move between RUNNABLE and RUNNING:

### Preemptive Scheduling

The OS can interrupt a RUNNING thread at any time (time slice expires):

```
Time slice = 10ms

Thread A: ████████░░░░░░░░████████░░░░░░░░
Thread B: ░░░░░░░░████████░░░░░░░░████████
               ↑                    ↑
          Preempted            Preempted
```

### Priority-Based Scheduling

Higher priority threads get more CPU time:

```
High priority:   ████████████████████░░░████████
Medium priority: ░░░░░░░░░░░░░░░░░░░██░░░░░░░░░
Low priority:    (starved — rarely gets CPU time)
```

**Danger:** Priority inversion — a high-priority thread waits for a lock held by a low-priority thread.

---

## Common Concurrency Issues by State

| Issue | Related State | Description |
|-------|--------------|-------------|
| Deadlock | BLOCKED | Two threads each waiting for a lock the other holds |
| Starvation | RUNNABLE | Thread never gets scheduled (low priority) |
| Livelock | RUNNING | Threads keep retrying but make no progress |
| Race condition | RUNNING | Multiple threads modify shared data without synchronization |

---

## Debugging Tips

1. **Thread dump:** Capture the state of all threads to identify which are BLOCKED and why
2. **Deadlock detection:** Look for circular BLOCKED dependencies
3. **Starvation:** Check if any thread stays in RUNNABLE too long without reaching RUNNING
4. **Resource leaks:** Threads stuck in WAITING forever (missing `notify()`)

---

## Takeaway

> Every thread moves through a defined lifecycle: NEW → RUNNABLE → RUNNING → (BLOCKED/WAITING) → TERMINATED. Understanding these states helps you reason about deadlocks, starvation, and race conditions. When debugging concurrency bugs, always ask: "What state is each thread in, and what is it waiting for?"
