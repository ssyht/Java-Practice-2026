# L6: Foundations – Why Generics and Generic Classes
**CMP_SC 3330 – Object-Oriented Programming**  
Dr. Ekincan Ufuktepe | University of Missouri – Columbia

---

## Overview
- Generic classes
- Generic methods
- Generics and inheritance
- Wildcards (`?`, `extends`, `super`)
- Writing flexible, reusable code

---

## Learning Goals
- Write generic classes
- Write generic methods
- Use wildcards safely
- Understand bounded generics
- Avoid common generic pitfalls

---

## Motivation: The Duplicate Code Problem

Without generics, creating a container for every type leads to massive code duplication:

```java
public class TaskBox {
    private Task item;
    // ...
}

public class StudentBox {
    private Student item;
    // ...
}
```

Every new type requires a brand new class — this doesn't scale.

---

## Generic Class Solution

Instead, use a **type parameter** `T`:

```java
public class Box<T> {

    private T item;

    public void set(T item) {
        this.item = item;
    }

    public T get() {
        return item;
    }
}
```

`T` is a placeholder that gets replaced with a real type when the class is instantiated.

### UML
```
┌─────────────────┐  [T]
│      Box        │
├─────────────────┤
│ - item : T      │
├─────────────────┤
│ + set(item : T) │
│ + get() : T     │
└─────────────────┘
```

---

## Using a Generic Class

No casting required — the compiler enforces type safety:

```java
Box<Task> taskBox = new Box<>();
taskBox.set(new Task("Study"));
Task task = taskBox.get();
```

---

## Multiple Type Parameters

A class can have more than one type parameter. This is exactly how `Map` works internally:

```java
public class Pair<K, V> {
    private K key;
    private V value;
    // ...
}
```

### Naming Convention

| Letter | Meaning |
|--------|---------|
| `T`    | Type    |
| `E`    | Element |
| `K`    | Key     |
| `V`    | Value   |

---

## Generic Type Safety

Generics catch errors at **compile time** rather than **runtime**.

```java
Box<Task> taskBox = new Box<>();
taskBox.set("Hello"); // COMPILE ERROR — String is not a Task
```

Runtime crashes are far more costly to debug and fix than compile-time errors.

---

## Generic Methods

A method can declare its own type parameter, independent of the class:

```java
public static <T> void print(T item) {
    System.out.println(item);
}
```

> Note: `<T>` appears **before** the return type, not as the return type itself.

### Calling a Generic Method

The same method works for any type — the compiler infers `T` from the argument:

```java
print("Hello");  // T inferred as String
print(123);      // T inferred as Integer
```

### UML
```
┌──────────────────────────────┐
│           Printer            │
├──────────────────────────────┤
│ + print<T>(item : T) : void  │
└──────────────────────────────┘
```

---

## Generics and Inheritance

### The Problem

Even though `Dog` is a subtype of `Animal`, **`List<Dog>` is NOT a subtype of `List<Animal>`**:

```java
List<Dog> dogs = new ArrayList<>();
List<Animal> animals = dogs; // COMPILE ERROR!
```

### Why Is This Dangerous?

If it were allowed, you could corrupt the list:

```java
List<Dog> dogs = new ArrayList<>();
List<Animal> animals = dogs;      // (hypothetically allowed)
animals.add(new Cat());           // Now dogs contains a Cat — broken!
```

This is a **type safety violation**, which is why Java forbids it.

---

## Generics and Interfaces

Interfaces can also be generic — a very common real-world pattern (think `List<T>`, `Set<T>`):

```java
public interface Repository<T> {
    void save(T item);
    T findById(int id);
}
```

### Implementation

```java
public class TaskRepository implements Repository<Task> {
    // ...
}
```

### UML
```
┌──────────────┐  [T]
│ «interface»  │
│  Repository  │
└──────┬───────┘
       △
       │
┌──────┴───────────┐
│  TaskRepository  │
└──────────────────┘
```

---

## Wildcards and Bounds

Wildcards add flexibility (or restrictions) when the exact generic type doesn't need to be fixed.

### 1. Unbounded Wildcard `<?>`

Accepts a list of **any** type:

```java
List<?> list;

void printList(List<?> list) { ... }
```

### 2. Upper Bounded Wildcard `<? extends T>`

Accepts `Animal` **or any subclass** — used for **reading/producing**:

```java
List<? extends Animal>

void printAnimals(List<? extends Animal> list) { ... }
```

### 3. Lower Bounded Wildcard `<? super T>`

Accepts `Dog` **or any superclass** — used for **writing/consuming**:

```java
List<? super Dog>

void addDog(List<? super Dog> list) { ... }
```

---

## PECS Rule

> **P**roducer **E**xtends, **C**onsumer **S**uper

| Situation | Wildcard | Operation |
|-----------|----------|-----------|
| Reading from a list | `<? extends T>` | Producer → use `extends` |
| Writing to a list   | `<? super T>`   | Consumer → use `super`   |

### Alternative wording
- `extends` = **read**
- `super` = **write**

---

## Common Mistakes

You **cannot add elements** to an `extends`-bounded list (the type is unknown at compile time):

```java
List<? extends Animal> animals = new ArrayList<>();
animals.add(new Dog()); // COMPILE ERROR
```

Use `<? super Dog>` when you need to insert elements.

---

## Summary

| Concept | Key Takeaway |
|---------|--------------|
| Generic classes | Reusable, type-safe containers using `<T>` |
| Generic methods | Methods with their own `<T>` before the return type |
| Generics & inheritance | `List<Dog>` is **not** a `List<Animal>` |
| Generics & interfaces | Interfaces can be generic (e.g., `Repository<T>`) |
| Wildcards | `<?>`, `<? extends T>`, `<? super T>` for flexible APIs |
| PECS | Producer Extends, Consumer Super |

---

## Up Next
**JUnit and Testing Generics**