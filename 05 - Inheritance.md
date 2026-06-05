---
title: "Inheritance & Method Overriding in C++"
series: "OOP Fundamentals"
order: 3
tags: [cpp, oop, inheritance, override, constructor-chaining]
---

# Inheritance & Method Overriding in C++

## What You'll Learn

How to model hierarchical relationships using inheritance, chain constructors from parent to child, and override methods to add specialized behavior.

---

## The Problem

You need to model different types of animals (dogs, birds) that share common properties (name, species, age) but also have unique attributes. Without inheritance, you'd duplicate the shared logic in every class.

## The Solution: Inheritance

A base `animal` class holds common attributes. `dog` and `bird` classes **inherit** from `animal` and extend it with their own unique properties.

```
animal (name, species, age)
├── dog   (+ breed)
└── bird  (+ color, can_fly)
```

---

## Key Concepts

### Inheritance Syntax

```cpp
class dog : public animal { ... };
```

`dog` is a **child class** (subclass) of `animal` (parent/base class). It inherits all public and protected members of `animal`.

### Constructor Chaining with Initializer List

```cpp
dog(string name, string specie, int age, string breed)
    : animal(name, specie, age)  // Call parent constructor FIRST
{
    this->breed = breed;  // Then set child-specific fields
}
```

**Why this matters:**
- `: animal(name, specie, age)` ensures the parent is fully initialized before the child
- The parent constructor runs first — this is constructor chaining
- `this->breed` disambiguates between the parameter and member variable

### Method Overriding

```cpp
void display_info() override {
    animal::display_info();   // Reuse parent's implementation
    cout << "Breed: " << breed << endl;  // Add child-specific info
}
```

**Key points:**
- `override` keyword tells the compiler this intentionally overrides a parent method
- `animal::display_info()` explicitly calls the parent's version — reuse, don't rewrite
- If the parent signature changes, the compiler catches the mismatch

---

## Code Walkthrough

### Dog Class

```cpp
dog myDog("Bolt", "Husky", 3, "Siberian Husky");
myDog.display_info();
```

**Output:**
```
Name: Bolt
Species: Husky
Age: 3 years
Breed: Siberian Husky
```

### Bird Class

```cpp
bird myBird("Tweety", "Canary", 2, "Yellow", true);
myBird.display_info();
```

**Output:**
```
Name: Tweety
Species: Canary
Age: 2 years
Color: Yellow
Can Fly: Yes
```

---

## Design Decisions

| Decision | Why |
|----------|-----|
| `public` inheritance | Child classes expose parent's public interface |
| Constructor chaining | Parent is always in a valid state before child initialization |
| `override` keyword | Compile-time safety — catches signature mismatches |
| Calling `animal::display_info()` | DRY — reuse parent logic instead of duplicating it |

---

## Common Pitfalls

| Mistake | Problem |
|---------|---------|
| Forgetting to call parent constructor | Parent members uninitialized — undefined behavior |
| Not using `override` | Silent bugs if parent method signature changes |
| Deep inheritance trees | Hard to trace behavior — prefer composition for complex cases |

---

## Takeaway

> Inheritance lets you **reuse** parent logic and **extend** it in child classes. Use `ParentClass::method()` inside overrides to avoid rewriting shared code. Always use `override` to make your intent explicit to the compiler.