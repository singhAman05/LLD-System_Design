---
title: "Concurrency vs Parallelism"
series: "Concurrency & Multithreading"
order: 2
tags: [concurrency, parallelism, multithreading, cpu]
---

# Concurrency vs Parallelism

## The Core Distinction

These two terms are often confused. Here's the precise difference:

| | Concurrency | Parallelism |
|---|---|---|
| **Definition** | Multiple tasks in progress at the same time | Multiple tasks executing at the same instant |
| **Requires** | Task management (single core is sufficient) | Multiple CPU cores or processors |
| **Focus** | Structure and design | Execution and speed |
| **Analogy** | One chef, multiple dishes | Multiple chefs, multiple dishes |

> **Concurrency** is about structure — how you organize tasks.
> **Parallelism** is about execution — how you speed them up.

---

## Visual Comparison

```
SEQUENTIAL (1 core, 1 task at a time):
Core 1: |████ Task A ████|████ Task B ████|████ Task C ████|

CONCURRENT (1 core, interleaved):
Core 1: |██A██|██B██|██C██|██A██|██B██|██C██|██A██|██B██|

PARALLEL (multiple cores, simultaneous):
Core 1: |████████████ Task A ████████████|
Core 2: |████████████ Task B ████████████|
Core 3: |████████████ Task C ████████████|

CONCURRENT + PARALLEL (multiple cores, interleaved):
Core 1: |██A██|██C██|██A██|██C██|
Core 2: |██B██|██D██|██B██|██D██|
```

---

## The Coffee Shop Analogy

### Sequential
One barista. Makes each drink completely before starting the next. Customers wait in a long queue.

### Concurrent
One barista. Starts steaming milk for order 1 → while milk steams, grinds beans for order 2 → pours order 1 → starts order 3. Tasks overlap in progress but only one hand does work at a time.

### Parallel
Three baristas. Each makes a different drink at the same time. Three drinks produced simultaneously.

### Concurrent + Parallel
Three baristas, each interleaving multiple orders. Maximum throughput.

---

## Technical Examples

### Concurrent, Not Parallel

A Node.js web server handling 1000 requests on a single thread using an event loop:

```javascript
// All requests are "in progress" but only one
// piece of JS executes at any instant
server.on('request', async (req, res) => {
    const data = await db.query(sql);  // Non-blocking I/O
    res.send(data);
});
```

The event loop handles I/O callbacks — while one request waits for the database, another request's callback runs.

### Parallel, Not Concurrent

A matrix multiplication where each core computes a different row independently:

```cpp
#pragma omp parallel for
for (int i = 0; i < N; i++) {
    for (int j = 0; j < N; j++) {
        C[i][j] = dot_product(A[i], B_col[j]);
    }
}
```

Each core has exactly one task — no interleaving, no switching.

### Both Concurrent and Parallel

A Java web server with a thread pool of 8 threads handling 1000 concurrent connections across 4 CPU cores:

- **Concurrent:** 1000 connections are all "in progress"
- **Parallel:** 4 cores execute threads simultaneously
- **Combined:** OS schedules 8 threads across 4 cores while managing 1000 connections

---

## When Each Matters

| Scenario | What you need | Why |
|----------|--------------|-----|
| Web server (many clients) | Concurrency | Thousands of connections, mostly waiting for I/O |
| Video encoding | Parallelism | CPU-intensive computation on independent frames |
| Game engine | Both | Rendering, physics, AI, input — all must progress simultaneously |
| Mobile app | Concurrency | Keep UI responsive during network calls (usually 1-2 cores available) |

---

## Performance Implications

### I/O-Bound Tasks → Concurrency Helps

```
Without concurrency: Download A (5s) → Download B (5s) = 10s
With concurrency:    Download A + B overlapping = ~5s
```

Adding more cores won't help — the bottleneck is network/disk, not CPU.

### CPU-Bound Tasks → Parallelism Helps

```
Without parallelism: Process 1M records on 1 core = 10s
With 4 cores:       Process 250K records per core = ~2.5s
```

Concurrency alone won't help here — you need actual simultaneous execution.

---

## Common Misconception

> "More threads = more speed"

**Wrong.** If you have 4 cores and create 100 threads for CPU-bound work, you get 4-way parallelism + 96 threads waiting for CPU time + context switching overhead. Performance may actually decrease.

**Rule of thumb:**
- I/O-bound → many concurrent tasks are fine (hundreds or thousands)
- CPU-bound → threads ≈ number of cores for optimal performance

---

## Takeaway

> Concurrency is a **design concept** (structuring programs to handle multiple tasks). Parallelism is an **execution model** (actually running things simultaneously on multiple cores). Good systems use both — concurrency for structure, parallelism for speed.
