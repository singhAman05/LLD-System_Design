# 📄 food.cpp — Food Order System (State Management)

**File:** [[food.cpp]] **Concept:** Classes, Encapsulation, State Management, Vectors

---

## 🧠 What Does This File Do?

Models a food ordering system. An `order` object holds customer info, a list of items, total price, and an order status (`is_placed`). Once placed, the order is locked — no more items can be added.

---

## 🔑 Key Concepts

### State Management with a Boolean Flag

`is_placed` is a `bool` flag that tracks the lifecycle of an order:

- `false` → order is open, items can be added
- `true` → order is placed, no modifications allowed

This pattern is common in real systems (e.g., "submitted", "locked", "published").

### `vector<string>` for Dynamic Lists

```cpp
vector<string> items;
```

A `vector` is like a dynamic array — it grows as you push items into it. Much more flexible than a fixed-size array.

---

## 🔍 Code Walkthrough

```cpp
void add_item(string item, double price) {
    if (is_placed) {
        cout << "Cannot add items to a placed order." << endl;
        return;  // Early return — guard clause
    }
    items.push_back(item);
    total_price += price;
}
```

> Guard clause prevents adding items to a finalized order.

```cpp
void place_order() {
    if (items.empty()) {
        cout << "Cannot place an empty order." << endl;
        return;
    }
    is_placed = true;
}
```

> Validates before committing state change — you can't place an empty order.

```cpp
for (const auto &item : items) {
    cout << item << ", ";
}
```

> **Range-based for loop** with `const auto &` — reads each item without copying it.

---

## 💡 Takeaway

> State flags (`bool is_placed`) are a simple and powerful way to model **lifecycle transitions** in a class. Guard clauses enforce valid state at every step.

---

**Tags:** #cpp #oop #encapsulation #state #vector #food