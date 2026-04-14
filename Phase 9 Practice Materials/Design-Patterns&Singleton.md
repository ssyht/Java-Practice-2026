# L9 – Design Patterns & Singleton
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe**

---

## What Are Design Patterns?

- **Proven solutions** to common, recurring software design problems
- Popularized by the **"Gang of Four" (GoF)**: Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides
- Goals:
  - Provide a **shared vocabulary** for developers
  - Improve **readability** and **maintainability**
  - Encourage **best practices**

---

## Why Use Design Patterns?

| Benefit | Meaning |
|---|---|
| **Reusability** | Don't reinvent the wheel |
| **Scalability** | Easier to expand software |
| **Maintainability** | Cleaner, more organized code |
| **Collaboration** | Shared understanding among teammates |

---

## Three Types of Design Patterns

### 1. Creational — *Object Creation*
Deal with **how** objects are created.
- Examples: **Singleton**, Factory Method, Builder

### 2. Structural — *Object Composition*
Deal with **how** objects are assembled/composed.
- Examples: Adapter, Composite, Decorator

### 3. Behavioral — *Object Interaction*
Deal with **how** objects communicate and share responsibility.
- Examples: Observer, **Strategy**, Command

---

## When to Use Design Patterns?

- When facing **recurring design problems**
- When designing **complex systems**
- When improving **flexibility and scalability**

> ⚠️ **Design patterns are guidelines, not strict rules.**  
> Overusing them leads to unnecessary complexity — sometimes called **"patternitis"**.  
> Always understand the problem *before* reaching for a pattern.

---

## Singleton Design Pattern

### Purpose
- Ensure a class has **only one instance**
- Provide a **global access point** to that instance
- Use case: managing configurations, logging, database connections

### The Problem It Solves (and Creates)
The Singleton actually solves **two problems at once**, which technically violates the **Single Responsibility Principle (SRP)**:
1. Ensure only **one** instance exists
2. Provide **global access** to it

### Solution Mechanics
1. Make the **constructor private** → prevents `new Singleton()` from outside
2. Create a **static method** (`getInstance()`) that acts as the constructor
3. On first call, it creates the object and saves it in a **static field**
4. All subsequent calls return the **same cached object**

### Java Implementation

```java
public class Singleton {
    // 1. Private static field holds the single instance
    private static Singleton instance;

    // 2. Private constructor prevents external instantiation
    private Singleton() {}

    // 3. Public static method provides global access
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();  // Created only once
        }
        return instance;
    }
}
```

### Usage Example

```java
Singleton s1 = Singleton.getInstance();
Singleton s2 = Singleton.getInstance();
System.out.println(s1 == s2); // true — same object!
```

---

## Singleton: Pros & Cons

| ✅ Pros | ❌ Cons |
|---|---|
| Guarantees only **one** instance | Violates **SRP** (two responsibilities) |
| **Global access point** | Can **mask bad design** (tight coupling) |
| Initialized **lazily** (on first request) | Needs special handling in **multithreaded** environments |
| | **Hard to unit test** (private constructor, can't mock easily) |

---

## Key Vocabulary

| Term | Definition |
|---|---|
| **Design Pattern** | Reusable solution to a common design problem |
| **Gang of Four (GoF)** | Authors who popularized the 23 classic patterns |
| **Creational Pattern** | Patterns about object creation |
| **Singleton** | Pattern ensuring exactly one class instance |
| **Static field** | Class-level variable shared across all instances |
| **Lazy initialization** | Object created only when first needed |
| **Patternitis** | Anti-pattern: overusing design patterns unnecessarily |