---
title: "Classes & Encapsulation in C++"
series: "OOP Fundamentals"
order: 2
tags: [cpp, oop, encapsulation, classes, constructor]
---

# Classes & Encapsulation in C++

## What You'll Learn

How to define a class that models a real-world entity with **private data** and **public methods** — the foundation of encapsulation in C++.

---

## The Problem

Without classes, you'd use loose variables and functions. Anyone can set `speed = -500` and there's no way to prevent invalid state. The program has no structure, no safety, and no scalability.

## The Solution: Encapsulation

A `class` bundles data and behavior together. You mark data as `private` (hidden) and expose controlled access through `public` methods.

```cpp
class car {
private:
    string brand;
    string model;
    int speed;

public:
    car(string &brand_name, string &model_name)
        : brand(brand_name), model(model_name), speed(0) {}

    void accelerate(int increment) { speed += increment; }

    void brake(int decrement) {
        speed -= decrement;
        if (speed < 0) speed = 0;  // Guard clause
    }

    void display_status() {
        cout << "Brand: " << brand << endl;
        cout << "Model: " << model << endl;
        cout << "Speed: " << speed << " km/h" << endl;
    }
};
```

---

## Key Concepts

### Data Hiding

`speed`, `brand`, `model` are `private` — they cannot be accessed or modified directly from outside the class. This prevents external code from putting the object into an invalid state.

### Constructor

```cpp
car(string &brand_name, string &model_name)
    : brand(brand_name), model(model_name), speed(0) {}
```

- Takes brand and model by **reference** (avoids copying overhead)
- Sets speed to `0` by default — a safe initial state
- Uses **member initializer list** for efficient initialization

### Public Interface

| Method | What it does |
|--------|-------------|
| `accelerate(int)` | Increases speed by given amount |
| `brake(int)` | Decreases speed, clamped to 0 (no negative speed) |
| `display_status()` | Prints brand, model, and current speed |

---

## Code Walkthrough

### Guard Clause Pattern

```cpp
void brake(int decrement) {
    speed -= decrement;
    if (speed < 0) speed = 0;  // Guard: speed can't go below 0
}
```

> This is a **guard clause** — a safety check that prevents the object from reaching an invalid state. The car can never have negative speed.

### Usage

```cpp
car myCar(brand, model);  // Constructor called — speed = 0
myCar.accelerate(50);     // speed = 50
myCar.brake(20);          // speed = 30
myCar.brake(100);         // speed = 0 (clamped, not -70)
```

---

## Design Decisions

| Decision | Why |
|----------|-----|
| `speed` is private | External code can't set it to invalid values |
| Constructor sets speed to 0 | Every car starts stationary — safe default |
| `brake()` clamps to 0 | Physical impossibility modeled correctly |
| Pass-by-reference in constructor | Avoids unnecessary string copies |

---

## Takeaway

> **Encapsulation = data hiding + controlled access.** The car's speed can't be set to -999 from outside — the class enforces its own invariants. This is the most fundamental principle of OOP.