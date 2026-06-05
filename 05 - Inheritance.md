# 📄 inheritance.cpp — Animal Hierarchy (Inheritance)

**File:** [[inheritance.cpp]] **Concept:** Inheritance, Method Overriding, Constructor Chaining

---

## 🧠 What Does This File Do?

Models an animal hierarchy. A base `animal` class holds common attributes (name, species, age). `dog` and `bird` classes **inherit** from `animal` and add their own unique attributes (breed, color, can_fly).

---

## 🔑 Key Concepts

### Inheritance Syntax

```cpp
class dog : public animal { ... };
```

`dog` is a **child class** (subclass) of `animal` (parent/base class). It inherits all public/protected members of `animal`.

### Constructor Chaining with Initializer List

```cpp
dog(string name, string specie, int age, string breed) : animal(name, specie, age)
{
    this->breed = breed;
}
```

- `: animal(name, specie, age)` calls the **parent constructor** first.
- Then the dog-specific field `breed` is set.
- `this->breed` disambiguates between the parameter `breed` and the member `breed`.

### Method Overriding

```cpp
void display_info() override {
    animal::display_info();   // Call parent version first
    cout << "Breed: " << breed << endl;  // Then add dog-specific info
}
```

- `override` keyword signals intentional override (compiler will warn if signature doesn't match).
- `animal::display_info()` explicitly calls the **parent's version** — useful to reuse rather than rewrite shared logic.

---

## 🔍 Code Walkthrough

**Hierarchy:**

```
animal (name, species, age)
├── dog   (+ breed)
└── bird  (+ color, can_fly)
```

```cpp
dog myDog("Bolt", "Husky", 3, "Siberian Husky");
myDog.display_info();
// Output:
// Name: Bolt
// Species: Husky
// Age: 3 years
// Breed: Siberian Husky
```

```cpp
bird myBird("Tweety", "Canary", 2, "Yellow", true);
myBird.display_info();
// Output:
// Name: Tweety
// Species: Canary
// Age: 2 years
// Color: Yellow
// Can Fly: Yes
```

---

## 💡 Takeaway

> Inheritance lets you **reuse** parent logic and **extend** it in child classes. Use `ParentClass::method()` inside overrides to avoid rewriting shared code.

---

**Tags:** #cpp #oop #inheritance #override #constructor-chaining