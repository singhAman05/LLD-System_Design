# 📄 bank_hierarchy.cpp — Bank Account Hierarchy

**File:** [[bank_hierarchy.cpp]] **Concept:** Inheritance, Virtual Methods, Override, `protected` Members

---

## 🧠 What Does This File Do?

Models a banking system. `BankAccount` is the base class with common deposit/withdraw/display logic. `SavingsAccount` and `CheckingAccount` inherit from it and each override `withdraw()` with their own rules.

---

## 🔑 Key Concepts

### `protected` vs `private`

```cpp
protected:
    string account_holder_name;
    double balance;
```

- `private` members are **not accessible** to child classes.
- `protected` members **are accessible** to child classes but still hidden from outside code.
- `balance` must be `protected` so `SavingsAccount` and `CheckingAccount` can read/modify it directly.

### Virtual Method Override

```cpp
virtual bool withdraw(double amount) { ... }  // Base: no overdraft, no minimum
```

Child classes override this with their own business rules:

- `SavingsAccount::withdraw()` — enforces a **$100 minimum balance**
- `CheckingAccount::withdraw()` — allows **overdraft** up to a limit

### `printf` for Formatting

```cpp
printf("%.2f", balance);
```

Used for clean 2-decimal-place output without `setprecision`.

---

## 🔍 Code Walkthrough

**Hierarchy:**

```
BankAccount (name, number, balance)
├── SavingsAccount  (+ interest_rate, min balance rule)
└── CheckingAccount (+ overdraft_limit)
```

```cpp
// SavingsAccount: balance can't go below $100
bool withdraw(double amount) override {
    if (amount > balance || balance - amount < 100) return false;
    balance -= amount;
    return true;
}
```

Alice has $1000. Withdraw $950 → balance would be $50 < $100 → **rejected**.

```cpp
// CheckingAccount: can overdraft up to limit
bool withdraw(double amount) override {
    if (amount > balance + overdraft_limit) return false;
    balance -= amount;
    return true;
}
```

Bob has $500, overdraft $300. Withdraw $700 → $700 ≤ $800 → **allowed**. Balance becomes -$200.

```cpp
void applyInterest() {
    balance += balance * interest_rate / 100;
}
```

Adds interest on current balance. E.g. 2% on $1000 → balance becomes $1020.

---

## 💡 Takeaway

> Use `protected` when child classes need direct access to parent data. Use `virtual` + `override` to give each subclass its own business rules while sharing a common interface.

---

**Tags:** #cpp #oop #inheritance #virtual #protected #bank