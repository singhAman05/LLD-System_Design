# 🗂️ C++ OOP Codebase — Overview

## What is this codebase?

A collection of C++ programs that explore **Object-Oriented Programming (OOP)** concepts through real-world-like examples. Each file is a standalone mini-project demonstrating one or more OOP principles.

---

## 🧠 Core Concepts Covered

|Concept|Files|
|---|---|
|Classes & Encapsulation|[[classes.cpp]], [[food.cpp]], [[temperature_sensor.cpp]], [[shoppingCart_system.cpp]]|
|Inheritance|[[inheritance.cpp]], [[bank_hierarchy.cpp]], [[notification_system.cpp]]|
|Polymorphism (Virtual Functions)|[[payments.cpp]], [[data_exporter.cpp]], [[discount_system.cpp]], [[shape_hierarchy.cpp]]|
|Abstract Classes (Pure Virtual)|[[payments.cpp]], [[data_exporter.cpp]], [[discount_system.cpp]]|

---

## 🗺️ Project Map

```
📁 codebase/
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

## 🔑 OOP Pillar Summary

### 1. Encapsulation

Bundling data (`private`) and methods (`public`) together. External code can only interact through defined interfaces. → See: [[classes.cpp]], [[food.cpp]]

### 2. Inheritance

A child class inherits properties and behaviours from a parent class using `: public ParentClass`. → See: [[inheritance.cpp]], [[bank_hierarchy.cpp]]

### 3. Polymorphism

Same function name, different behaviour depending on which class it belongs to. Achieved via `virtual` functions. → See: [[payments.cpp]], [[shape_hierarchy.cpp]]

### 4. Abstraction

Hiding implementation details. Pure virtual functions (`= 0`) force subclasses to implement specific behaviour. → See: [[data_exporter.cpp]], [[discount_system.cpp]]

---

## 🔗 All Files
- [[classes.cpp]]
- [[food.cpp]]
- [[temperature_sensor.cpp]]
- [[shoppingCart_system.cpp]]
- [[inheritance.cpp]]
- [[bank_hierarchy.cpp]]
- [[notification_system.cpp]]
- [[payments.cpp]]
- [[data_exporter.cpp]]
- [[discount_system.cpp]]
- [[shape_hierarchy.cpp]]