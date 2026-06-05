# 📄 data_exporter.cpp — Data Exporter (Abstract Class + Template Method)

**File:** [[data_exporter.cpp]] **Concept:** Abstract Class, Pure Virtual, Template Method Pattern, Shared Validation

---

## 🧠 What Does This File Do?

Exports data in different formats (CSV, JSON). The base `DataExporter` class provides a shared `validate()` method and declares `exportData()` as pure virtual. Subclasses implement the format-specific export logic but all reuse the same validation.

---

## 🔑 Key Concepts

### Shared Logic in Base Class

```cpp
bool validate(const vector<string> &data) {
    if (data.empty()) return false;
    cout << "Validation passed. Exporting " << data.size() << " records." << endl;
    return true;
}
```

`validate()` is a **concrete method** in the abstract base class. All exporters call it before exporting. This is the **Template Method pattern** — the base class defines the algorithm skeleton, subclasses fill in the specifics.

### Pure Virtual Export

```cpp
virtual void exportData(const vector<string> &data) = 0;
```

Forces all subclasses to implement how they export — but not how they validate.

### CSV Exporter

```cpp
void exportData(const vector<string> &data) override {
    if (!validate(data)) { cout << "Invalid data." << endl; return; }
    cout << "CSV: ";
    for (const auto &item : data) cout << item << ",";
    cout << endl;
}
```

Outputs: `CSV: Alice,Bob,Charlie,`

### JSON Exporter

```cpp
// Outputs: JSON: ["Alice","Bob","Charlie"]
for (size_t i = 0; i < data.size(); ++i) {
    cout << "\"" << data[i] << "\"";
    if (i != data.size() - 1) cout << ",";  // No trailing comma
}
```

Uses `size_t` index loop to detect the last element (to avoid a trailing comma). Note the use of `\"` to escape double quotes in the output.

---

## 🔍 Code Walkthrough

```
csv.exportData({"Alice", "Bob", "Charlie"})
→ Validation passed. Exporting 3 records.
→ CSV: Alice,Bob,Charlie,

json.exportData({"Alice", "Bob", "Charlie"})
→ Validation passed. Exporting 3 records.
→ JSON: ["Alice","Bob","Charlie"]

csv.exportData({})
→ Invalid data. Cannot export.
```

---

## 💡 Takeaway

> Put **shared logic** in the base class, **format-specific logic** in subclasses. This avoids duplicating validation code across all exporters. Adding a new format (e.g., XML) means just writing one new class — no touching existing code.

---

**Tags:** #cpp #oop #abstract-class #pure-virtual #template-method #exporter #json #csv