# Week 4: Inheritance, Interfaces, and Polymorphism

## 📚 Overview

This week we explore three fundamental concepts in Object-Oriented Programming:
- **Reusing behavior responsibly** through inheritance
- **Replacing conditionals with polymorphism**
- **Designing for extension** using interfaces
- **Understanding when inheritance helps and when it hurts**

---

## 🎯 Learning Goals

By the end of this module, you should be able to:
- Explain inheritance and polymorphism
- Design and implement interfaces
- Apply polymorphism to remove conditionals
- Identify bad inheritance designs
- Understand Liskov Substitution intuitively

---

## 💡 Big Idea

> **Write code that works with what objects DO, not what they ARE.**

This principle is the foundation of polymorphic design.

---

## 1. Inheritance Fundamentals

### What is Inheritance?

Inheritance allows one class (child/subclass) to acquire the properties and methods of another class (parent/superclass).

### Why Use Inheritance?

- **Avoid duplication** - Share common code
- **Share common behavior** - Implement once, use everywhere
- **Model "is a" relationships** - Dog IS AN Animal

### Basic Syntax

```java
public class Animal {
    void speak() { }
}

public class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("Woof");
    }
}
```

**Key Terms:**
- `Animal` is the **parent/super class**
- `Dog` is the **child class**
- `extends` keyword establishes the inheritance relationship

### What Inheritance Gives You

1. **Field and method reuse** - Child classes inherit all public/protected members
2. **Method overriding** - Child classes can provide specialized implementations
3. **Subtype polymorphism** - Use child objects where parent type is expected

---

## 2. Method Overriding

### The `@Override` Annotation

```java
@Override
public void speak() {
    System.out.println("Woof");
}
```

**What does `@Override` do?**
- It's a Java **annotation** that indicates you're intentionally overriding a parent method
- **Helps the compiler detect errors** like:
  - Incorrect method names
  - Mismatched parameters
  - Methods that don't actually override anything
- **Improves code readability** - Makes your intention clear to other developers
- **Generates compiler errors** if the method doesn't actually override anything

**Best Practice:** Always use `@Override` when overriding methods!

---

## 3. The "Is A" Test

Before using inheritance, ask: **"Is A [child] an [parent]?"**

### ✅ Good Examples:
- A Dog **IS AN** Animal ✓
- A Warrior **IS A** GameCharacter ✓

### ❌ Bad Examples:
- A Car **IS NOT A** Wheel ✗
- A Square **IS NOT ALWAYS A** Rectangle ✗ (LSP violation)

**Rule:** If the "is a" relationship doesn't hold naturally, don't use inheritance!

---

## 4. Abstract Classes

### What are Abstract Classes?

Abstract classes represent **incomplete concepts** that cannot be instantiated directly.

```java
public abstract class Shape {
    public abstract double area();
}

public class Square extends Shape {
    @Override
    public double area() {
        // TODO: Implement area calculation
        return 0;
    }
}
```

### Key Characteristics:

- **Cannot be instantiated** - You can't do `new Shape()`
- **May contain abstract methods** - Methods without a body
- **Represents incomplete concepts** - Partial implementation
- **"Abstract"** means *"not fully defined"*, not "imaginary"

### Abstract Methods

```java
public abstract class GameCharacter {
    // Abstract method - no body
    public abstract void attack();
    
    // Concrete method - has a body
    public void takeDamage(int damage) {
        // Implementation here
    }
}
```

**Abstract methods are a promise that all child classes MUST keep.**

### Example: Game Character System

```
GameCharacter (Abstract)
├── name: String
├── health: int
├── attack(): void (abstract)
└── takeDamage(damage: int): void

    ↑ extends          ↑ extends
    
Warrior              Mage
└── attack(): void   └── attack(): void
```

---

## 5. Interfaces

### What is an Interface?

An interface is a **contract** that defines what a class can do, without specifying how.

```java
public interface Payable {
    double calculatePay();
}
```

### Key Characteristics:

- **Define behavior contracts** - Specify what, not how
- **No state (mostly)** - Cannot have instance variables (with some exceptions in modern Java)
- **Support multiple inheritance** - A class can implement many interfaces
- **They are NOT classes!**

### Interface Example

```java
public interface Payable {
    double calculatePay();
}
```

**See what I meant by abstract methods being like interface methods?**
- Methods in an interface **don't have a body**
- They define what needs to be done, not how

---

## 6. Interface vs Abstract Class

### When to Use What?

| Interface | Abstract Class |
|-----------|---------------|
| **What you can do** | **What you are** |
| Behavior contract | Partial implementation |
| No state | Can have state |
| Multiple inheritance | Single inheritance |
| More flexible | More structured |

**Prefer interfaces for flexibility!**

### Quick Decision Guide

**Should it be an interface?**
- ✅ Flyable - describes a capability
- ✅ Serializable - describes a capability
- ❌ Bird - describes what something IS
- ✅ Walkable - describes a capability

**Rule of thumb:** If it ends in "-able", it's probably an interface!

---

## 7. Naming Interfaces

### The "-able" Convention

We generally use words ending in **"…able"** for interfaces:

- `Flyable` - Can fly
- `Walkable` - Can walk
- `Serializable` - Can be serialized
- `Runnable` - Can be run
- `Comparable` - Can be compared
- `Cloneable` - Can be cloned

This naming makes it clear that the interface describes a **capability** or **behavior**.

---

## 8. Polymorphism

### What Is Polymorphism?

Polymorphism means "many forms" - the ability of different objects to respond to the same message in different ways.

**Core Principles:**
- **One interface** - Single method signature
- **Many implementations** - Different behaviors
- **Same message** - Call the same method
- **Different behavior** - Get different results

### Polymorphism in Code

```java
Animal a = new Dog();
a.speak(); // Outputs: "Woof"
```

**What's happening here?**
- Variable `a` is declared as type `Animal`
- Object is actually a `Dog`
- When we call `speak()`, Java calls the `Dog` version
- This is **polymorphism** - modifying common behavior to something specific

---

## 9. Why Polymorphism Matters

### Benefits:

1. **Extensible systems** - Easy to add new types
2. **Fewer if statements** - Behavior lives in objects
3. **Cleaner designs** - Less conditional logic

### Before Polymorphism (Bad)

```java
if (type.equals("DOG")) {
    bark();
} else if (type.equals("CAT")) {
    meow();
}
```

### After Polymorphism (Good)

```java
animal.speak();
```

**The difference?** 
- No conditionals needed
- Behavior is determined by the object itself
- Adding new animal types doesn't require changing existing code

---

## 10. Replacing Conditionals with Polymorphism

### The Problem: Conditional-Based Design

```java
if (type.equals("DOG")) {
    bark();
} else if (type.equals("CAT")) {
    meow();
}
```

**Issues:**
- Hard to extend
- Fragile logic
- Long if chains

### The Solution: Polymorphic Design

```java
animal.speak();
```

**Advantages:**
- No conditionals
- Behavior moved into objects
- Open for extension

---

## 11. Liskov Substitution Principle (LSP)

### What is LSP?

**Subtypes must be usable where their base type is expected, without breaking behavior.**

In simple terms: **If it looks like a duck, but breaks your code, it's not a duck.**

### LSP Violation Example

```java
public class Square extends Rectangle {
    public void setWidth(double w) {
        width = height = w;  // ❌ Violates Rectangle's contract
    }
}
```

**Why is this bad?**
```java
Rectangle r = new Square();
r.setWidth(5);
r.setHeight(3);
// Expected: 5 x 3 rectangle
// Got: 3 x 3 square
// Behavior is SURPRISING and BROKEN
```

### Consequences of LSP Violations:

- **Surprising behavior** - Code doesn't work as expected
- **Broken assumptions** - Preconditions/postconditions violated
- **Hidden bugs** - Errors appear in unexpected places

---

## 12. Composition Over Inheritance

### The Principle

**Prefer combining objects over extending classes.**

### Why?

- **Combine objects** - Build complex behavior from simple parts
- **Delegate behavior** - Pass responsibility to other objects
- **More flexible designs** - Easier to change and test

### Composition Example

```java
public class Engine { 
    // Engine implementation
}

public class Car {
    private Engine engine;  // Car HAS AN Engine
    
    public void start() {
        engine.start();  // Delegate to engine
    }
}
```

**Note:** Car **HAS AN** Engine, not **IS AN** Engine!

---

## 13. Practical Application: Task System

### Starting Problem

You have tasks with:
- Different priorities
- Different completion behaviors
- Conditional-based logic everywhere

### Solution: Interface-Based Design

```java
public interface Task {
    void complete();
}

public class SimpleTask implements Task {
    @Override
    public void complete() {
        System.out.println("Task completed!");
    }
}

public class TimedTask implements Task {
    @Override
    public void complete() {
        System.out.println("Timed task completed at: " + new Date());
    }
}
```

### Using Polymorphism

```java
// No conditionals needed!
for (Task task : tasks) {
    task.complete();
}
```

**Benefits:**
- No conditionals
- Behavior moved into objects
- Open for extension - just add new Task implementations

---

## 14. UML Diagram: Interface with Multiple Implementations

```
┌─────────────────────┐
│   «interface»       │
│       Task          │
├─────────────────────┤
│ + execute(): void   │
└─────────────────────┘
          △
          │ implements
    ┌─────┴─────┐
    │           │
┌───────┐   ┌────────┐
│EmailTask│   │FileTask│
├───────┤   ├────────┤
│execute()│   │execute()│
└───────┘   └────────┘

        ↑
        │ depends on
        │
┌─────────────────────┐
│      Client         │
├─────────────────────┤
│ - task: Task        │
├─────────────────────┤
│ + runTask(): void   │
└─────────────────────┘
```

**Key Points:**
- Client depends on the **abstraction** (Task interface)
- Multiple implementations provide different behaviors
- Adding new task types doesn't affect Client code

---

## 15. Design Tradeoffs

### Polymorphic Design

**Costs:**
- More classes to manage
- More files in your project
- Initial complexity

**Benefits:**
- Better structure and organization
- Easier to make changes later
- More maintainable code
- Less fragile than conditionals

**The Payoff:** Short-term complexity for long-term maintainability.

---

## 16. Common Polymorphism Mistakes

### 1. Empty Base Methods

```java
public class Animal {
    public void speak() {
        // Empty - forces subclasses to override
    }
}
```

**Better:** Use an abstract method or interface

### 2. Inheriting for Convenience

```java
public class Stack extends ArrayList {
    // Using inheritance just to reuse ArrayList methods
}
```

**Problem:** Stack IS NOT AN ArrayList - use composition instead

### 3. Too Deep Hierarchies

```java
Animal → Mammal → Carnivore → Feline → DomesticCat → Tabby
```

**Problem:** Deep hierarchies are hard to understand and maintain

**Better:** Keep hierarchies shallow (2-3 levels max)

---

## 17. The Polymorphism Decision Rule

### Quick Test:

> **If you need an `if` statement every time you add a new class, polymorphism is calling!**

### Example:

```java
// Adding a new animal requires modifying this code
if (animal instanceof Dog) {
    // Dog behavior
} else if (animal instanceof Cat) {
    // Cat behavior
} else if (animal instanceof Bird) {  // New addition
    // Bird behavior
}
```

**This is a code smell!** Use polymorphism instead:

```java
// Adding a new animal requires NO changes here
animal.speak();
```

---

## 🎯 Key Takeaways

1. **Inheritance shares behavior** - Use it to avoid duplication
2. **Interfaces define roles** - What objects can do
3. **Polymorphism simplifies design** - Eliminates conditionals
4. **LSP matters** - Subtypes must behave correctly
5. **Prefer composition** - Often safer than inheritance
6. **Use the "is a" test** - Before choosing inheritance
7. **Name interfaces with "-able"** - Makes purpose clear

---

## 📖 Summary

### When to Use What?

| Scenario | Use |
|----------|-----|
| Share implementation between related classes | **Abstract Class** |
| Define a contract for unrelated classes | **Interface** |
| Need multiple inheritance | **Interface** |
| Want to eliminate conditionals | **Polymorphism** |
| Reuse behavior without "is a" | **Composition** |

### The Golden Rules

1. **Write code that works with what objects DO**
2. **Replace type-checking with polymorphism**
3. **Favor composition over inheritance**
4. **Keep hierarchies shallow**
5. **Always respect the Liskov Substitution Principle**

---

## 🚀 Up Next

In the next module, we'll explore:
- **Collections** - Working with groups of objects
- **Equality** - Comparing objects correctly
- **Why polymorphism matters even more** - Advanced applications

---

## 💻 Practice Exercises

### Exercise 1: Shape Hierarchy
Create an abstract `Shape` class with:
- Abstract method `area()`
- Abstract method `perimeter()`
- Implement `Circle`, `Rectangle`, `Triangle`

### Exercise 2: Payment System
Design a payment system using interfaces:
- Interface `Payable` with method `calculatePay()`
- Implement for `CreditCardPayment`, `PayPalPayment`, `CashPayment`
- Write a method that processes any `Payable` object

### Exercise 3: Refactor Conditionals
Take this code and refactor using polymorphism:
```java
if (employee.getType().equals("HOURLY")) {
    pay = hours * rate;
} else if (employee.getType().equals("SALARIED")) {
    pay = salary / 12;
} else if (employee.getType().equals("COMMISSIONED")) {
    pay = basePay + (sales * commissionRate);
}
```

---

## 📚 Additional Resources

- **Effective Java** by Joshua Bloch - Chapter on inheritance
- **Head First Design Patterns** - Strategy and Template Method patterns
- **Clean Code** by Robert Martin - Polymorphism chapter
- **SOLID Principles** - Especially the 'L' (Liskov Substitution)

---

**Remember:** The goal isn't just to use inheritance and polymorphism, but to use them **well** and at the **right times**. Sometimes the simplest solution is the best solution!

Happy coding! 🎉