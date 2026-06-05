---
title: "Shape Hierarchy — Dynamic Dispatch & Virtual Functions"
series: "Exercises"
order: 9
tags: [cpp, oop, polymorphism, virtual-functions, dynamic-memory]
---

# shape_hierarchy.cpp — Shape Hierarchy (Polymorphism + Dynamic Dispatch)

**Concept:** Virtual Functions, Polymorphic Arrays, Dynamic Memory, `M_PI`

---

## What Does This File Do?

Defines a shape hierarchy. `Shape` is the base class with virtual `area()` and `perimeter()`. `circle` and `rectangle` override them. An array of base-class pointers holds different shape types — and polymorphism ensures each calls its own math.

---

## Key Concepts

### Virtual (Not Pure Virtual) with Default

```cpp
virtual double area() { return 0; }
virtual double perimeter() { return 0; }
```

These are `virtual` but **not** pure virtual — they have a default (return 0). So `Shape` itself can be instantiated (though it would be meaningless to do so). Subclasses override them with real formulas.

### `describe()` as Template Method

```cpp
void describe() {
    cout << "Shape: " << name;
    printf("%.2f", area());       // Calls overridden area()
    printf("%.2f", perimeter());  // Calls overridden perimeter()
}
```

Calls `area()` and `perimeter()` which dispatch to the correct subclass via vtable. `printf("%.2f", ...)` formats to 2 decimal places.

### `M_PI` for π

```cpp
double area() override { return M_PI * radius * radius; }
double perimeter() override { return 2 * M_PI * radius; }
```

`M_PI` is a constant from `<cmath>` (value ≈ 3.14159265...). Available via `bits/stdc++.h`.

### Polymorphic Array of Pointers

```cpp
Shape *shapes[2];
shapes[0] = new circle("Circle1", 5);
shapes[1] = new rectangle("Rectangle1", 4, 6);

for (int i = 0; i < 2; i++) {
    shapes[i]->describe();  // Calls correct subclass's area/perimeter
}
```

The array holds `Shape*` pointers, but they point to actual `circle` and `rectangle` objects. C++ dispatches the right `area()` at runtime via the **vtable** (virtual function table).

### Manual Memory Cleanup

```cpp
for (int i = 0; i < 2; i++) delete shapes[i];
```

`new` allocates on the heap — `delete` must be called to free it. Without this, you get a **memory leak**. Modern C++ prefers `unique_ptr` to automate this.

---

## Code Walkthrough

```
Circle1 (r=5):
  Area = π × 5² = 78.54
  Perimeter = 2π × 5 = 31.42

Rectangle1 (4×6):
  Area = 4 × 6 = 24.00
  Perimeter = 2(4+6) = 20.00
```

---

## Takeaway

> `Shape*` arrays + `virtual` functions = true polymorphism. The loop treats every shape the same, but each one computes its own geometry. This is **runtime polymorphism** via vtable dispatch.

---

**Tags:** #cpp #oop #polymorphism #virtual #dynamic-memory #shapes #vtable #M_PI