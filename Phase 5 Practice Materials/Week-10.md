# L5: Collections and Introduction to Generics
**CMP_SC 3330 – Object-Oriented Programming**  
Dr. Ekincan Ufuktepe | University of Missouri – Columbia

---

## Learning Goals

- Use core collection types
- Explain `equals` and `hashCode`
- Understand why generics exist
- Avoid type safety bugs
- Design safer object collections

---

## Collections Framework

### Why Collections Exist

Programs rarely manage just one object. Examples: many tasks, many students, many bank accounts. We need structure.

### The Problem With Arrays

Arrays are fixed-size, hard to manage, and have limited functionality.

```java
Task[] tasks = new Task[10]; // stuck at 10 forever
```

### The Collections Framework

Provides dynamic resizing, powerful operations, and standardized interfaces.

**Key types:**
- `ArrayList` – ordered, dynamic list
- `HashSet` – unordered, no duplicates
- `HashMap` – key-value pairs

### Core Interfaces Hierarchy

```
Collection (interface)
├── List (interface)
│   └── ArrayList (class)
└── Set (interface)
    └── HashSet (class)
```

> **Note:** `Map` is separate from `Collection` but still part of the framework.

### ArrayList Example

```java
List<Task> tasks = new ArrayList<>();

tasks.add(new Task("Study"));
tasks.add(new Task("Sleep"));
```

### Set Example

```java
Set<String> names = new HashSet<>();

names.add("Alice");
names.add("Alice"); // duplicate — ignored
// Output contains only one "Alice"
```

### Map Example

```java
Map<Integer, Task> tasks = new HashMap<>();
// Stores key-value pairs (e.g., task ID → Task object)
```

### Collections Store References, Not Copies

Collections hold references to objects, not copies of them. Remember: reference vs. value semantics.

---

## `equals` and `hashCode`

### Why Equality Matters

How does `Set` detect duplicates? It uses `equals`. If you don't override it, you get the wrong behavior.

### Default `equals` Behavior

By default, `equals` compares **memory addresses** (reference equality):

```java
new Task("Study") != new Task("Study") // true — different objects!
```

Two logically equal objects appear different, which breaks collection behavior.

### Overriding `equals`

Override `equals` to define **logical equality** (e.g., two Tasks are equal if their descriptions are equal).

**Proper implementation:**

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Task)) return false;
    Task other = (Task) o;
    return description.equals(other.description);
}
```

### The `equals` Contract

`equals` must be:
- **Reflexive** – `x.equals(x)` is always true
- **Symmetric** – if `x.equals(y)`, then `y.equals(x)`
- **Transitive** – if `x.equals(y)` and `y.equals(z)`, then `x.equals(z)`
- **Consistent** – repeated calls return the same result

### `hashCode` Purpose

Used by hash-based collections (`HashSet`, `HashMap`) to find objects quickly (bucket lookup).

**The rule:** If two objects are equal via `equals`, they **must** return the same `hashCode`.

```java
@Override
public int hashCode() {
    return description.hashCode();
}
```

### How `HashSet` Uses Both

1. `hashCode()` → finds the right bucket
2. `equals()` → confirms the object is actually the same

### Common Mistake: Override `equals` But Not `hashCode`

If you override only `equals`, `HashSet` breaks — equal objects land in different buckets and duplicates slip through.

---

## Generics and Type Safety

### Why Generics Exist

- Prevent **runtime** type errors
- Enable **compile-time** safety

### The Problem: Raw Collections

```java
List tasks = new ArrayList(); // raw — no type info

tasks.add(new Task("Study"));
tasks.add("Hello"); // compiles fine... but disaster ahead

Task t = (Task) tasks.get(1); // 💥 ClassCastException at runtime
```

### The Solution: Parameterized Collections

```java
List<Task> tasks = new ArrayList<>();

tasks.add(new Task("Study"));
tasks.add("Hello"); // ❌ Compiler error — caught immediately!
```

The compiler now enforces that only `Task` objects go in.

### Benefits of Generics

| Without Generics | With Generics |
|-----------------|---------------|
| Runtime crashes | Compile-time errors |
| Manual casting required | No casting needed |
| Unclear intent | Self-documenting code |

### Generics Remove the Need for Casting

```java
// Without generics:
Task t = (Task) tasks.get(0); // must cast

// With generics:
Task t = tasks.get(0); // no cast needed
```

---

## Summary

| Concept | Purpose |
|---------|---------|
| **Collections** | Manage groups of objects dynamically |
| **`equals`** | Define logical equality between objects |
| **`hashCode`** | Enable correct behavior in hash-based collections |
| **Generics** | Provide compile-time type safety |

> **Always override both `equals` and `hashCode` together** when using objects in `HashSet` or `HashMap`.

---

*Up next: Generics deep dive*
