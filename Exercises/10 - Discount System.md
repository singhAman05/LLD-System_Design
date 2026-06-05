# 📄 discount_system.cpp — Discount System (Polymorphism + Strategy Pattern)

**File:** [[discount_system.cpp]] **Concept:** Abstract Class, Polymorphism, Strategy Pattern, `snprintf` for Label Formatting

---

## 🧠 What Does This File Do?

A flexible discount system supporting three types of discounts: percentage off, flat amount off, and buy-one-get-one-free. Each is its own class. The `orderProcessor` applies any discount without knowing its type — it just calls `describe()`.

---

## 🔑 Key Concepts

### Abstract Base: `discountSystem`

```cpp
class discountSystem {
protected:
    string label;
public:
    virtual double apply(double price) = 0;  // Pure virtual — must be implemented
    void describe(double originalPrice) {
        double discountedPrice = apply(originalPrice);  // Calls overridden version
        // prints: "label: $original -> $discounted"
    }
};
```

`describe()` is a **concrete method** that internally calls the abstract `apply()`. This is the **Template Method** pattern again — the base class controls the flow, subclasses supply the calculation.

### `snprintf` for Dynamic Labels

```cpp
char buf[32];
snprintf(buf, sizeof(buf), "%.1f", percentage);
this->label = string(buf) + "% off";
```

`snprintf` formats a number into a char buffer safely (prevents buffer overflow). Then it's converted to `std::string`. Used to build labels like `"20.0% off"` or `"$15.0 off"`.

### Three Discount Strategies

|Class|Logic|Example|
|---|---|---|
|`percentageDiscount`|`price * (1 - pct/100)`|20% off $999.99 → $799.99|
|`flatDiscount`|`max(0, price - amount)`|$15 off $49.99 → $34.99|
|`buyOneGetOneFree`|`price / 2`|$79.98 → $39.99 (effectively)|

### `max(0.0, price - amount)`

Prevents flat discounts from going negative — e.g., $15 off a $5 item = $0, not -$10.

### Strategy Pattern

```cpp
void processOrder(const string &itemName, double price, discountSystem *discount) {
    discount->describe(price);  // Works for ANY discount type
}
```

`orderProcessor` doesn't know (or care) what kind of discount is passed — it just calls `describe()`. You can swap strategies at runtime.

---

## 🔍 Code Walkthrough

```
Laptop $999.99 → 20% off: $999.99 -> $799.99
Headphones $49.99 → $15 off: $49.99 -> $34.99
Keyboard $79.98 → BOGO: $79.98 -> $39.99
```

---

## 💡 Takeaway

> The **Strategy Pattern** = define a family of algorithms (discounts), encapsulate each one, make them interchangeable. Code that uses them (`orderProcessor`) doesn't need to change when new strategies are added.

---

**Tags:** #cpp #oop #abstract-class #polymorphism #strategy-pattern #discount #snprintf