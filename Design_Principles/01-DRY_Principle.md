## What is the DRY Principle?

**DRY (Don't Repeat Yourself)** is a software design principle that focuses on eliminating duplication of knowledge and logic within a codebase.

The goal is to ensure that every piece of business logic has a **single source of truth**, making systems easier to maintain, extend, and debug.

------

## 🔑 Key Principles

### 1. Single Source of Truth

A business rule should be defined only once.

Instead of duplicating values and logic across multiple classes or functions, centralize them in a single location.

---

### 2. Avoid Logic Duplication

Repeated business logic increases maintenance cost.

If a rule changes, every duplicated implementation must be updated, increasing the risk of bugs.

---

### 3. Extract Common Functionality

Shared behavior should be moved into:

- Functions
- Helper classes
- Base classes
- Interfaces

This improves readability and maintainability.

---

### 4. Use Abstractions Carefully

Abstractions should eliminate meaningful duplication.

Creating unnecessary layers of inheritance or overly generic classes can make the code harder to understand.

---

### 5. Balance DRY with Simplicity

Applying DRY aggressively can lead to over-engineering.

A good design balances:

- DRY
- KISS (Keep It Simple)
- YAGNI (You Aren't Gonna Need It)
- SRP (Single Responsibility Principle)

---

## ⚠️ Common Mistakes

### Over-Abstraction

Creating complex hierarchies to eliminate very small amounts of duplication.

```cpp
AbstractManagerFactoryProvider
```

for only a few repeated lines of code.

This often hurts readability more than it helps.

---

### Premature Optimization

Extracting shared code before duplication actually becomes a problem.

Not all repetition requires abstraction.

---

### Duplicating Business Rules

The most dangerous form of duplication is repeating business knowledge in multiple locations.

When requirements change, inconsistencies can appear across the system.

---

## 🎯 Benefits of DRY

- Easier maintenance
- Faster updates
- Better code reuse
- Reduced bugs
- Improved readability
- Cleaner architecture

---

## 🔗 Related Principles

- [[02-KISS_Principle]]
- [[03-YAGNI_Principle]]
- [[04-SOLID_Principles]]

---

## 📝 Interview Takeaway

When designing a system:

1. Identify repeated business logic.
2. Extract common behavior into reusable components.
3. Maintain a single source of truth.
4. Avoid unnecessary abstractions.
5. Keep the design simple and maintainable.

The objective is not to eliminate every duplicate line of code, but to eliminate duplicated knowledge and business rules.