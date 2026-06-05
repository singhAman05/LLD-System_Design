---
title: "Shopping Cart System � Maps & Discount Logic"
series: "Exercises"
order: 3
tags: [cpp, oop, unordered-map, encapsulation]
---

# shoppingCart_system.cpp — Shopping Cart System

**Concept:** Classes, `unordered_map`, State Flags, Discount Logic

---

## What Does This File Do?

A shopping cart that lets you add items (by price), apply a discount code, calculate total, and check out. Once checked out, the cart is locked — nothing can be added or discounted further.

---

## Key Concepts

### `unordered_map<string, double>`

```cpp
unordered_map<string, double> items;
```

Maps item names to their prices. Adding the same item again **adds to** its price value (not replacing). This acts like a running total per item.

### Discount Code System

Only one discount code (`"SAVE10"`) is valid, and it can only be applied **once** before checkout. The flag `discount_code != ""` ensures no double-dipping.

### Checkout Lock

`is_checked_out` is a boolean that, once `true`, blocks all mutations to the cart — a pattern called **immutability after commit**.

### `fixed` and `setprecision(2)`

```cpp
cout << fixed << setprecision(2);
```

Ensures prices always display with exactly 2 decimal places (e.g., `$926.98` not `$926.982`).

---

## Code Walkthrough

```cpp
void addItem(string item, double quantity) {
    if (is_checked_out) { return; }  // Locked after checkout
    items[item] += quantity;          // Accumulates price per item
}
```

```cpp
bool applyDiscount(string code) {
    if (is_checked_out) return false;    // Too late
    if (discount_code != "") return false; // Already applied
    if (code == "SAVE10") {
        discount_code = code;
        return true;
    }
    return false;
}
```

```cpp
double getTotal() {
    double total = 0;
    for (auto &item : items) total += item.second; // Sum all prices
    if (discount_code == "SAVE10") total *= 0.9;   // 10% off
    return total;
}
```

**Flow in `main()`:**

```
Add Laptop ($999.99) + Mouse ($29.99) → Total: $1029.98
Apply SAVE10 → Total: $926.98
Try SAVE10 again → false (already applied)
Checkout → cart locked
Add Keyboard → rejected
Total still: $926.98
```

---

## Takeaway

> Boolean state flags are great for simple lifecycle control. The cart can only move **forward** (open → checked out), never backward — just like real e-commerce systems.

---

**Tags:** #cpp #oop #unordered_map #state #discount #shopping