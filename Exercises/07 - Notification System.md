# 📄 notification_system.cpp — Notification System

**File:** [[notification_system.cpp]] **Concept:** Inheritance, Polymorphism, `vector` of Pointers, Runtime Dispatch

---

## 🧠 What Does This File Do?

A notification system that supports multiple delivery channels — Email, SMS, and WhatsApp. All share a common `notification_system` base class. A `notify_user` manager class holds a list of notifications and displays them all, regardless of type.

---

## 🔑 Key Concepts

### Polymorphism via `virtual`

```cpp
virtual void display_notification() { ... }
```

Each subclass overrides this with extra details (email address, phone number, etc.). When called through a base pointer, C++ dispatches to the **correct subclass version** at runtime.

### `vector<notification_system*>` — Collection of Base Pointers

```cpp
vector<notification_system *> notifications;
```

Stores pointers to the base class, but they can point to any subclass (`email_notification`, `sms_notification`, etc.). This is the classic **polymorphic collection** pattern.

```cpp
void display_all_notifications() {
    for (auto notification : notifications) {
        notification->display_notification();  // Calls the correct subclass version
    }
}
```

### `notification_system::display_notification()` — Calling Parent in Override

```cpp
void display_notification() override {
    notification_system::display_notification();  // Print common fields
    cout << "Email Address: " << email_address << endl;  // Then add specific field
}
```

---

## 🔍 Code Walkthrough

**Hierarchy:**

```
notification_system (name, timestamp, message)
├── email_notification    (+ email_address)
├── sms_notification      (+ phone_number)
└── whatsapp_notification (+ whatsapp_number)
```

**In `main()`:**

```cpp
// Create different notification types
email_notification *email = new email_notification(...);
sms_notification *sms = new sms_notification(...);

// Add to manager
user_notifications.add_notification(email);
user_notifications.add_notification(sms);

// Display all — polymorphism handles the rest
user_notifications.display_all_notifications();
```

Each notification prints its common info first, then its specific field. The `notify_user` class doesn't need to know what type each notification is — polymorphism handles dispatch automatically.

> ⚠️ **Memory note:** `new` is used here but `delete` is never called. In production, you'd want to manage this with smart pointers (`unique_ptr`) or explicit cleanup.

---

## 💡 Takeaway

> A `vector<BaseClass*>` + `virtual` functions = powerful **open/closed** design. You can add new notification types (push notification, Slack, etc.) without changing `notify_user` at all.

---

**Tags:** #cpp #oop #polymorphism #virtual #inheritance #notifications #vector-of-pointers