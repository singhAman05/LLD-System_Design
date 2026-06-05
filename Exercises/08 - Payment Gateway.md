---
title: "Payment Gateway � Abstract Classes & Dependency Injection"
series: "Exercises"
order: 6
tags: [cpp, oop, abstract-class, dependency-injection, polymorphism]
---

# payments.cpp — Payment Gateway (Abstract Class + Dependency Injection)

**Concept:** Abstract Class, Pure Virtual Functions, Dependency Injection, Polymorphism

---

## What Does This File Do?

Defines a pluggable payment system. `Payment_Gateway` is an **abstract interface** — it forces any payment provider (PayPal, Stripe) to implement `process_payment()`. The `checkout_service` accepts any gateway and works with all of them the same way.

---

## Key Concepts

### Abstract Class with Pure Virtual Function

```cpp
class Payment_Gateway {
public:
    virtual ~Payment_Gateway() {}                    // Virtual destructor
    virtual void process_payment(double amount) = 0; // Pure virtual — must be overridden
};
```

- `= 0` makes this a **pure virtual function** — no default implementation.
- Any class with a pure virtual function is an **abstract class** — you **cannot** instantiate it directly.
- The `virtual` destructor ensures derived class destructors are called correctly when deleting via base pointer.

### Concrete Implementations

```cpp
class PayPal : public Payment_Gateway {
public:
    void process_payment(double amount) override {
        cout << "Processing PayPal payment of $" << amount << endl;
    }
};
```

`PayPal` and `Stripe` both **implement** the contract defined by `Payment_Gateway`.

### Dependency Injection

```cpp
class checkout_service {
private:
    Payment_Gateway *payment_gateway;  // Holds a pointer to the interface
public:
    checkout_service(Payment_Gateway *gateway) : payment_gateway(gateway) {}
    void complete_checkout(double amount) {
        payment_gateway->process_payment(amount);  // Calls whichever gateway was injected
    }
};
```

`checkout_service` doesn't care **which** gateway it uses — it just knows it has a `Payment_Gateway`. You pass the concrete gateway in at construction time. This is **dependency injection**.

---

## Code Walkthrough

```cpp
PayPal paypal_gateway;
Stripe stripe_gateway;

checkout_service paypal_checkout(&paypal_gateway);  // Inject PayPal
checkout_service stripe_checkout(&stripe_gateway);  // Inject Stripe

paypal_checkout.complete_checkout(100.0);  // "Processing PayPal payment of $100"
stripe_checkout.complete_checkout(200.0);  // "Processing Stripe payment of $200"
```

---

## Takeaway

> Abstract classes define **contracts**. Dependency injection + polymorphism = code that is **open to extension** (add new gateways) but **closed to modification** (don't touch `checkout_service`). This is the Open/Closed Principle.

---

**Tags:** #cpp #oop #abstract-class #pure-virtual #dependency-injection #polymorphism #payments