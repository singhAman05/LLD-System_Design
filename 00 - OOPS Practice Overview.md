---
title: "C++ OOP Codebase — Overview"
series: "OOP Fundamentals"
order: 1
tags: [cpp, oop, encapsulation, inheritance, polymorphism, abstraction]
---

# C++ OOP Codebase — Overview

## What is this codebase?

A structured collection of C++ programs that explore **Object-Oriented Programming (OOP)** concepts through real-world-like examples. Each file is a standalone mini-project demonstrating one or more OOP principles — from basic encapsulation to full polymorphic systems.

> Think of this as a practical companion to theory. Every concept has working code you can run, break, and extend.

---

## Core Concepts Covered

| Concept | Files | Key Idea |
|---------|-------|----------|
| Classes & Encapsulation | `classes.cpp`, `food.cpp`, `temperature_sensor.cpp`, `shoppingCart_system.cpp` | Data hiding + controlled access |
| Inheritance | `inheritance.cpp`, `bank_hierarchy.cpp`, `notification_system.cpp` | Code reuse through parent-child relationships |
| Polymorphism (Virtual Functions) | `payments.cpp`, `data_exporter.cpp`, `discount_system.cpp`, `shape_hierarchy.cpp` | Same interface, different behavior |
| Abstract Classes (Pure Virtual) | `payments.cpp`, `data_exporter.cpp`, `discount_system.cpp` | Enforcing contracts on subclasses |

---

## Project Map

```
codebase/
├── classes.cpp              → Basic class with encapsulation (Car)
├── food.cpp                 → Order management system
├── temperature_sensor.cpp   → Data validation + stats
├── shoppingCart_system.cpp  → Shopping cart with discount logic
├── inheritance.cpp          → Animal → Dog/Bird hierarchy
├── bank_hierarchy.cpp       → BankAccount → Savings/Checking
├── notification_system.cpp  → Notification → Email/SMS/WhatsApp
├── payments.cpp             → Abstract payment gateway
├── data_exporter.cpp        → Abstract data exporter (CSV/JSON)
├── discount_system.cpp      → Abstract discount system
└── shape_hierarchy.cpp      → Abstract shape hierarchy (Circle/Rectangle)
```

---

## The Four Pillars of OOP

### 1. Encapsulation

Bundling data (`private`) and methods (`public`) together. External code can only interact through defined interfaces.

**Why it matters:** Prevents invalid state. A car's speed can't be set to -999 from outside — the class enforces its own rules.

→ See: `classes.cpp`, `food.cpp`

### 2. Inheritance

A child class inherits properties and behaviors from a parent class using `: public ParentClass`. Enables code reuse without duplication.

**Why it matters:** You write the shared logic once (in the parent) and extend it in each child — not copy-paste it.

→ See: `inheritance.cpp`, `bank_hierarchy.cpp`

### 3. Polymorphism

Same function name, different behavior depending on which class it belongs to. Achieved via `virtual` functions and runtime dispatch.

**Why it matters:** You can write code that works with _any_ implementation — add new types without modifying existing logic.

→ See: `payments.cpp`, `shape_hierarchy.cpp`

### 4. Abstraction

Hiding implementation details. Pure virtual functions (`= 0`) force subclasses to implement specific behavior — the base class defines _what_ must happen, subclasses define _how_.

**Why it matters:** You define a contract that all implementations must follow, enabling plug-and-play architecture.

→ See: `data_exporter.cpp`, `discount_system.cpp`

---

## How to Use This Codebase

1. **Read the concept note** — Understand the principle being demonstrated
2. **Read the code** — Follow the structure and logic
3. **Run it** — Compile and execute to see the output
4. **Break it** — Remove a `virtual` keyword, make something public that shouldn't be — see what fails
5. **Extend it** — Add a new subclass, a new method, a new feature

---

## Related Series

- **Design Principles** — DRY, KISS, YAGNI, SOLID
- **Concurrency & Multithreading** — Threads, processes, synchronization
- **Exercises** — Complete implementations applying these concepts