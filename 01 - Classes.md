# 📄 classes.cpp — Car Class (Encapsulation)

**File:** [[classes.cpp]] **Concept:** Classes, Encapsulation, Constructors, Methods

---

## 🧠 What Does This File Do?

Defines a `car` class that models a real car — with a brand, model, and speed. You can accelerate, brake, and display the car's status. It's a clean example of **encapsulation**: the `speed` is hidden from the outside world and can only be changed through controlled methods.

---

## 🔑 Key Concepts

### Encapsulation

- `speed`, `brand`, `model` are `private` — they cannot be accessed or modified directly from outside the class.
- Public methods (`accelerate`, `brake`, `display_status`) act as the **interface** to safely interact with the data.

### Constructor

```cpp
car(string &brand_name, string &model_name)
```

- Takes brand and model by **reference** (no copying overhead).
- Sets speed to `0` by default — a safe initial state.

### Methods

|Method|What it does|
|---|---|
|`accelerate(int)`|Increases speed by given amount|
|`brake(int)`|Decreases speed, but clamps to 0 (no negative speed)|
|`display_status()`|Prints brand, model, and current speed|

---

## 🔍 Code Walkthrough

```cpp
void brake(int decrement) {
    speed -= decrement;
    if (speed < 0) speed = 0;  // Guard: speed can't go below 0
}
```

> This is a **guard clause** — a safety check that prevents invalid state.

```cpp
car myCar(brand, model);  // Constructor called
myCar.accelerate(50);     // speed = 50
myCar.brake(20);          // speed = 30
```

---

## 💡 Takeaway

> Encapsulation = **data hiding + controlled access**. The car's speed can't be set to -999 from outside — the class enforces its own rules.

---

**Tags:** #cpp #oop #encapsulation #classes #constructor