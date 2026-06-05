---
title: "Temperature Sensor — Data Validation"
series: "Exercises"
order: 2
tags: [cpp, oop, validation, stl-algorithms]
---

# temperature_sensor.cpp — Temperature Sensor (Data Validation)

**Concept:** Classes, Input Validation, STL Algorithms, Rounding

---

## What Does This File Do?

Simulates a temperature sensor that collects readings, validates them (rejects out-of-range values), and computes statistics like average and count.

---

## Key Concepts

### Input Validation

Only readings between `-50°C` and `150°C` are accepted. Anything outside that range is silently ignored (or can be logged).

### STL `accumulate`

```cpp
#include <numeric>
double sum = accumulate(readings.begin(), readings.end(), 0.0);
```

`std::accumulate` sums all values in a range. The `0.0` starting value ensures floating-point precision (not integer sum).

### Rounding to 2 Decimal Places

```cpp
return round(sum / readings.size() * 100.0) / 100.0;
```

Multiply by 100 → round → divide by 100. A classic trick to round to 2 decimal places without any special library.

---

## Code Walkthrough

```cpp
void addReading(double temp) {
    if (temp >= -50 && temp <= 150) {
        readings.push_back(temp);
    }
    // else: silently discard (commented-out log)
}
```

> Validates range before accepting. `200.0` would be rejected here.

```cpp
double getAverage() {
    if (readings.empty()) return 0;  // Guard: no division by zero
    double sum = accumulate(readings.begin(), readings.end(), 0.0);
    return round(sum / readings.size() * 100.0) / 100.0;
}
```

**In `main()`:**

```
Readings added: 22.5, 23.1, 200.0 (rejected), -10.0
Count: 3
Average: (22.5 + 23.1 + -10.0) / 3 = 11.87
```

---

## Takeaway

> Always validate input at the boundary of your class. Once bad data gets in, it's hard to clean up. **Fail early, fail loudly** (or silently, depending on your design).

---

**Tags:** #cpp #oop #validation #stl #accumulate #sensor