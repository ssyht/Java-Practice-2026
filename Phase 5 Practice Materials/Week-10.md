# L5: Collections and Introduction to Generics
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe | University of Missouri – Columbia**

---

## Why Collections Exist

Programs rarely manage just one object. We need structures to handle many tasks, many students, many bank accounts, etc.

**The problem with arrays:**
- Fixed size — you must declare how many elements upfront
- Hard to manage
- Limited built-in functionality

```java
Task[] tasks = new Task[10]; // stuck at 10!
```

---

## The Collections Framework

Java's Collections Framework solves array limitations by providing:
- **Dynamic resizing** — grows as needed
- **Powerful built-in operations** — add, remove, search, etc.
- **Standardized interfaces** — consistent API across types

### Core Interface Hierarchy

```
Collection (interface)
├── List (interface)
│   └── ArrayList (class)
└── Set (interface)
    └── HashSet (class)

Map (separate hierarchy)
└── HashMap (class)
```

---

## Core Collection Types

### ArrayList — ordered, dynamic list

```java
List<Task> tasks = new ArrayList<>();
tasks.add(new Task("Study"));
tasks.add(new Task("Sleep"));
```

### HashSet — no duplicates allowed

```java
Set<String> names = new HashSet<>();
names.add("Alice");
names.add("Alice"); // ignored — only one "Alice" in the set
```

### HashMap — key-value pairs

```java
Map<Integer, Task> tasks = new HashMap<>();
tasks.put(1, new Task("Study"));
tasks.get(1); // retrieves the Task with key 1
```

> **Note:** Collections store *references*, not copies of objects.

---

## equals and hashCode

### Why Equality Matters

How does `HashSet` detect duplicates? It uses `equals`. The problem: **default `equals` compares memory addresses**, not content.

```java
new Task("Study") != new Task("Study") // two different objects in memory!
```

This means two logically identical tasks would both be added to a `HashSet` — breaking the "no duplicates" guarantee.

### The equals Contract

When you override `equals`, it must be:
- **Reflexive** — `a.equals(a)` is always true
- **Symmetric** — if `a.equals(b)`, then `b.equals(a)`
- **Transitive** — if `a.equals(b)` and `b.equals(c)`, then `a.equals(c)`
- **Consistent** — repeated calls return the same result

### Proper equals Implementation

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;               // same reference? definitely equal
    if (!(o instanceof Task)) return false;   // wrong type? not equal
    Task other = (Task) o;
    return description.equals(other.description); // compare meaningful fields
}
```

### hashCode

Hash-based collections (`HashSet`, `HashMap`) use `hashCode` to find the right "bucket" before calling `equals`.

**The golden rule:** If two objects are equal (`equals` returns true), they **must** have the same `hashCode`.

```java
@Override
public int hashCode() {
    return description.hashCode();
}
```

### How HashSet Uses Both

1. Computes `hashCode` → finds the bucket
2. Calls `equals` → confirms if it's actually a duplicate

**Common bug:** Overriding `equals` but *not* `hashCode` breaks `HashSet` — equal objects land in different buckets and duplicates slip through.

---

## Generics and Type Safety

### The Problem with Raw Collections

Without generics, you can accidentally add the wrong type:

```java
List tasks = new ArrayList();  // raw type — no type checking!
tasks.add(new Task("Study"));
tasks.add("Hello");            // a String sneaks in...

Task t = (Task) tasks.get(1); // CRASH at runtime! ClassCastException
```

This is a **runtime failure** — the bug only appears when the code runs.

### Generics Fix This

By parameterizing the collection with a type, the **compiler catches mistakes early**:

```java
List<Task> tasks = new ArrayList<>();
tasks.add(new Task("Study"));
tasks.add("Hello"); // COMPILER ERROR — caught before the program runs!
```

### Benefits of Generics

- **Type safety** — errors caught at compile time, not runtime
- **No casting needed** — `tasks.get(0)` returns a `Task`, not `Object`
- **Self-documenting code** — `List<Task>` clearly expresses intent

---

## Summary

| Concept | Key Idea |
|---|---|
| Collections | Manage groups of objects with dynamic sizing and rich operations |
| `ArrayList` | Ordered list, allows duplicates, dynamic sizing |
| `HashSet` | Unordered, no duplicates, uses `equals` + `hashCode` |
| `HashMap` | Key-value pairs, fast lookup |
| `equals` | Defines logical equality; must follow the contract |
| `hashCode` | Must be consistent with `equals`; used for bucket lookup |
| Generics | Enforce type safety at compile time; eliminate unsafe casts |

> **Next up:** Generics deep dive