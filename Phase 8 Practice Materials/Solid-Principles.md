# SOLID Principles

**CMP_SC 3330 – Object-Oriented Programming**
**Dr. Ekincan Ufuktepe | University of Missouri – Columbia**

---

## 1. What Is SOLID?

SOLID is a set of five design principles for writing clean, maintainable, and extensible object-oriented code. They were introduced by **Robert C. Martin** ("Uncle Bob") in his article *"Principles of Object-Oriented Design"* in the early 2000s.

The principles form the acronym **S.O.L.I.D.**:

| Letter | Principle |
|--------|-----------|
| **S** | Single Responsibility Principle |
| **O** | Open/Closed Principle |
| **L** | Liskov Substitution Principle |
| **I** | Interface Segregation Principle |
| **D** | Dependency Inversion Principle |

SOLID fits alongside related software philosophies:
- **YAGNI** – "You Aren't Gonna Need It" (don't over-engineer)
- **KISS** – "Keep It Simple, Stupid"
- **Vertical Slice** – deliver thin, end-to-end features
- **Big Ball of Mud** – the anti-pattern you're trying to avoid

---

## 2. Single Responsibility Principle (SRP)

> **"A class should have one, and only one, reason to change."**

### The Core Idea

Every class should be responsible for exactly **one thing**. If a class handles database access, UI rendering, *and* business logic, then changes to any one of those areas force you to touch that class — which is risky and messy.

Each distinct responsibility is called an **axis of change**. A class that crosses multiple axes of change is fragile.

### Real-World Analogy

Think of robots on an assembly line. Each robot is specialized for one task (welding, painting, inspection). This makes them:
- Easy to maintain
- Easy to upgrade individually
- Easy to replace without affecting others

A Swiss Army knife can do many things, but it's not great at any of them.

### How to Apply SRP

1. Think about how the program actually works.
2. State responsibilities as generally as possible.
3. Keep information about one thing in one place.
4. Responsibilities can be shared or delegated across classes.
5. A class with no responsibilities is likely superfluous.
6. Refactor whenever a class gets too complicated.
7. Break up big, complex classes.
8. Avoid centralized "intelligence" — it's inflexible.
9. Avoid complexity in the graph of interacting objects.
10. Poor responsibility assignments lead to **fragile systems**.

### How TDD Supports SRP

When writing unit tests, you focus on testing **one specific behavior**. If a class does too many things, it becomes hard to write isolated tests. This pressure naturally pushes you toward smaller, single-purpose classes.

---

## 3. Open/Closed Principle (OCP)

> **"Software entities should be open for extension, but closed for modification."** – Bertrand Meyer

### The Core Idea

Once a class is working and tested, you should be able to **add new behavior without editing the existing code**. New features come from extending (subclassing, implementing interfaces), not from modifying working code.

There are two interpretations:

| Version | Meaning |
|---------|---------|
| **Meyer's OCP** | Implementation is only modified to fix bugs; new features require new classes |
| **Polymorphic OCP** | Use interfaces and abstract classes so behavior can be swapped without modifying existing code |

### Real-World Analogy

You can swap the lens on an SLR camera or screw on a filter — without sawing off the old lens. The camera body (existing code) is **closed for modification**, but the lens system is **open for extension**.

### Practical Guidance

- Keep all member variables `private`
- Avoid global variables
- Program to interfaces, not concrete classes

### How TDD Supports OCP

When requirements change, TDD encourages you to **add new tests** for new behavior rather than modify existing passing tests. This maps directly to the OCP — the old tests (and the old code they test) remain untouched.

> **"All systems change during their life cycles. This must be borne in mind when developing systems expected to last longer than the first version."** – Ivar Jacobson

---

## 4. Liskov Substitution Principle (LSP)

> **"Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program."**

### The Core Idea

If `Dog` extends `Animal`, then anywhere your code uses an `Animal`, you should be able to drop in a `Dog` and everything should still work correctly. Subclasses must **honor the contract** of their parent class — they can't silently change behavior in ways that break callers.

Formally: *Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it.*

### Real-World Analogy

Any base-10 calculator — whether it's a cheap $2 model or a graphing calculator — should return `4` when you press `2 + 2 =`. The original functionality is preserved as you build on it.

### What Violates LSP?

- A subclass throws an exception for a method that the parent supports
- A subclass ignores or contradicts a parent's postcondition
- Code that checks `instanceof` to handle subtypes differently (this is a sign something is wrong)

Avoid **Run-Time Type Information (RTTI)** — checking the concrete type at runtime with `instanceof` is usually a LSP violation waiting to happen.

### How TDD Supports LSP

When you write tests against an interface or base class, those same tests should pass for any subclass. If a subclass breaks existing tests, you've found an LSP violation early.

---

## 5. Interface Segregation Principle (ISP)

> **"Clients should not be forced to depend upon interfaces that they do not use."**

### The Core Idea

Prefer many small, **client-specific interfaces** over one large, general-purpose interface.

If an interface has 10 methods and a class only needs 3 of them, that class is still forced to implement (or stub out) the other 7. That's unnecessary coupling. Split the interface into focused roles.

### Real-World Analogy

Imagine a device that outputs HDMI Audio/Video being plugged into a Digital Optical sound player. The mismatch is painful. If the interface had been more focused (audio-only vs. video+audio), the integration would be cleaner.

### Practical Guidance

- Design by contract (DbC) — define what each interface promises, nothing more
- Design *to* interfaces — depend on the role, not the concrete class
- If a class is forced to implement methods it doesn't use, the interface is too fat — split it

### How TDD Supports ISP

Unit tests for specific behaviors naturally push you toward smaller, focused interfaces. Testing becomes manageable when an interface has only the methods relevant to one role. Large, bloated interfaces make tests complicated and hard to mock.

---

## 6. Dependency Inversion Principle (DIP)

> **"High-level modules should not depend upon low-level modules. Both should depend upon abstractions."**
> **"Abstractions should not depend upon details. Details should depend upon abstractions."**

### The Core Idea

Don't let high-level business logic depend directly on low-level implementation details (like a specific database, a specific file reader, etc.). Instead, both should depend on an **abstraction** (an interface).

This gives you **loose coupling** — you can swap out the low-level implementation without touching the high-level logic.

### Real-World Analogy

A screwdriver bit doesn't care what brand of screwdriver handle it's attached to — as long as the connection standard (the abstraction) matches. The bit (low-level) and the handle (high-level) both depend on the standard (abstraction), not on each other directly.

### Key Concepts

| Term | Meaning |
|------|---------|
| **Loose Coupling** | Components depend on abstractions, not concrete classes |
| **Dependency Injection** | Dependencies are passed in from outside (constructor, method) rather than created internally |
| **Inversion of Control (IoC)** | The control of creating dependencies is moved out of the class |

### Example Pattern

Instead of:

```java
// BAD: high-level class directly creates its dependency
public class OrderService {
    private MySQLDatabase db = new MySQLDatabase();  // tightly coupled
}
```

Do this:

```java
// GOOD: depend on an abstraction; inject the dependency
public class OrderService {
    private Database db;  // abstraction

    public OrderService(Database db) {  // injected from outside
        this.db = db;
    }
}
```

Now `OrderService` works with any `Database` implementation — MySQL, PostgreSQL, an in-memory test database, etc.

### How TDD Supports DIP

When you depend on interfaces rather than concrete classes, you can **mock** those dependencies in tests. This makes unit tests fast and isolated — they don't need a real database or network connection to run. TDD naturally pushes you toward this loosely coupled design.

---

## 7. SOLID and TDD — The Big Picture

Every SOLID principle reinforces (and is reinforced by) **Test-Driven Development**:

| Principle | TDD Connection |
|-----------|----------------|
| **SRP** | Hard-to-test classes signal too many responsibilities |
| **OCP** | New tests for new behavior; existing tests stay green |
| **LSP** | Subclass tests reuse parent class tests; violations fail early |
| **ISP** | Small interfaces are easier to mock and test |
| **DIP** | Dependency injection enables mocking in unit tests |

---

## Summary

| Principle | One-Line Summary |
|-----------|-----------------|
| **S** – Single Responsibility | One class, one reason to change |
| **O** – Open/Closed | Extend behavior without modifying existing code |
| **L** – Liskov Substitution | Subtypes must be drop-in replacements for their base types |
| **I** – Interface Segregation | Many focused interfaces beat one fat interface |
| **D** – Dependency Inversion | Depend on abstractions; inject your dependencies |