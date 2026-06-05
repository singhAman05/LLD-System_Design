# 🗂️ SOLID Principles

## What are SOLID Principles?

**SOLID** is a collection of five Object-Oriented Design principles that help developers build software that is:

- Easier to maintain
- Easier to extend
- Easier to test
- Easier to scale

The principles were popularized by **Robert C. Martin (Uncle Bob)** and serve as guidelines for creating flexible and maintainable software systems.

## 🧠 SOLID Overview

| Principle | Full Form                       | Goal                                     |
| --------- | ------------------------------- | ---------------------------------------- |
| S         | Single Responsibility Principle | One responsibility per class             |
| O         | Open-Closed Principle           | Extend without modifying                 |
| L         | Liskov Substitution Principle   | Child classes should behave like parents |
| I         | Interface Segregation Principle | Small focused interfaces                 |
| D         | Dependency Inversion Principle  | Depend on abstractions                   |

## 🗺️ Principle Map

```text
SOLID
├── SRP → Single Responsibility Principle
├── OCP → Open-Closed Principle
├── LSP → Liskov Substitution Principle
├── ISP → Interface Segregation Principle
└── DIP → Dependency Inversion Principle
```

# 🔑 S — Single Responsibility Principle (SRP)

### Definition

A class should have **one responsibility** and therefore **one reason to change**.

### Problem

When a class handles multiple responsibilities:

- Changes become risky
- Bugs become harder to isolate
- Features become tightly coupled

### Goal

Separate responsibilities so that changes in one area do not affect unrelated functionality.

### Example

❌ Bad

```text
Employee
├── Calculate Salary
├── Generate Report
└── Send Email
```

✅ Good

```text
Employee
SalaryCalculator
ReportGenerator
EmailService
```

### Benefits

- Better maintainability
- Easier testing
- Lower coupling
- Cleaner design

# 🔑 O — Open-Closed Principle (OCP)

### Definition

Software entities should be:

- Open for Extension
- Closed for Modification

### Problem

Modifying existing code increases the risk of introducing bugs.

### Goal

Add new behavior without changing existing, working code.

### Example

❌ Bad

```cpp
if(type == "PDF")
    exportPDF();
else if(type == "CSV")
    exportCSV();
```

✅ Good

```cpp
class Exporter {
public:
    virtual void exportData() = 0;
};
```

Create new exporter implementations instead of modifying existing code.

### Benefits

- Safer feature additions
- Reduced regression bugs
- Easier scalability

# 🔑 L — Liskov Substitution Principle (LSP)

### Definition

A child class should be able to replace its parent class without breaking program behavior.

### Problem

If subclasses behave differently than expected, polymorphism becomes unreliable.

### Goal

Ensure parent and child classes can be used interchangeably.

### Example

✅ Valid

```text
Coffee
└── Cappuccino
```

A Cappuccino is a Coffee.

❌ Invalid

```text
Coffee
└── Water
```

Water is not Coffee.

### Classic Violation

```text
Bird
├── Fly()
└── Penguin
```

Penguins cannot fly.

### Benefits

- Reliable inheritance
- Predictable behavior
- Safer polymorphism

# 🔑 I — Interface Segregation Principle (ISP)

### Definition

Clients should not be forced to depend on methods they do not use.

### Problem

Large interfaces force classes to implement unnecessary functionality.

### Goal

Split large interfaces into smaller, focused interfaces.

### Example

❌ Bad

```cpp
class Worker {
public:
    virtual void work() = 0;
    virtual void eat() = 0;
};
```

A robot worker doesn't need `eat()`.

✅ Good

```cpp
class Workable {
public:
    virtual void work() = 0;
};

class Eatable {
public:
    virtual void eat() = 0;
};
```

### Benefits

- Cleaner APIs
- Less coupling
- Better flexibility

# 🔑 D — Dependency Inversion Principle (DIP)

### Definition

- High-level modules should not depend on low-level modules
- Both should depend on abstractions

### Key Components

| Component | Meaning |
|------------|----------|
| High-Level Module | Business logic |
| Low-Level Module | Implementation details |
| Abstraction | Interface / Contract |
| Detail | Concrete implementation |

### Problem

```cpp
class OrderService {
    MySQLDatabase db;
};
```

Business logic becomes tightly coupled to a specific database.

### Goal

Reduce coupling by introducing abstractions.

### Example

✅ Good

```cpp
class Database {
public:
    virtual void save() = 0;
};

class MySQLDatabase : public Database {};
class PostgreSQLDatabase : public Database {};

class OrderService {
    Database* db;
};
```

### Benefits

- Easier testing
- Easier replacement of implementations
- Better flexibility
- Lower coupling

## 🎯 SOLID Summary

| Principle | Goal |
|------------|--------|
| SRP | One responsibility |
| OCP | Extend without modifying |
| LSP | Maintain substitutability |
| ISP | Small focused interfaces |
| DIP | Depend on abstractions |

## 🔗 Related Notes

- [[01-DRY_Principle]]
- [[02-KISS_Principle]]
- [[03-YAGNI_Principle]]

## 📝 Interview Takeaway

When designing systems:

- Give every class a single responsibility
- Extend behavior without modifying existing code
- Ensure child classes can replace parent classes
- Keep interfaces small and focused
- Depend on abstractions instead of implementations

Following SOLID leads to software that is:

- Easier to maintain
- Easier to test
- Easier to extend
- Easier to understand

> SOLID is not a strict set of rules. It is a collection of design guidelines that help create flexible and maintainable software systems.