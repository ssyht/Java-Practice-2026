# CS 3330: Object-Oriented Programming — Final Exam Review
## Topics: SOLID Principles (L8) → Exception Handling (L15)
### University of Missouri – Columbia | Dr. Ekincan Ufuktepe

---

## L8 / EXTRA — SOLID Principles

SOLID is an acronym for five key OOP design principles that help build maintainable, flexible, scalable software.

---

### S — Single Responsibility Principle (SRP)
> *"A class should have only one reason to change."*

- Every class should do **one thing** and do it well.
- If a class handles multiple concerns (e.g., data storage AND formatting AND emailing), it has multiple reasons to change → violates SRP.
- **Fix:** Split into separate classes, each with one responsibility.

**Example violation:** A `Student` class that also generates PDF reports.  
**Fix:** Separate into `Student` and `StudentReportGenerator`.

---

### O — Open/Closed Principle (OCP)
> *"Software entities should be open for extension, but closed for modification." — Bertrand Meyer*

- You should be able to **add new behavior** without **changing existing code**.
- Achieved via **interfaces**, **abstract classes**, and **polymorphism**.
- **Meyer's OCP:** Modify implementation only to fix bugs; new features → new class.
- **Polymorphic OCP:** All member variables should be private; avoid global variables.

**Example:** Instead of adding `if/else` to a `Shape` class for new shapes, define a `Shape` interface and let each shape implement it.

---

### L — Liskov Substitution Principle (LSP)
> *"Subtypes must be substitutable for their base types."*

- If `S` is a subtype of `T`, you should be able to use `S` anywhere `T` is used **without breaking the program**.
- Subclasses must **honor the contract** of the parent class.
- Violations often occur when a subclass overrides a method in a way that breaks expected behavior.

**Classic violation:** `Square extends Rectangle` — setting width changes height, which breaks `Rectangle`'s contract.

---

### I — Interface Segregation Principle (ISP)
> *"Many client-specific interfaces are better than one general-purpose interface."*

- Clients should **not be forced** to depend on interfaces they do not use.
- Split large "fat" interfaces into smaller, focused ones.

**Example violation:** A `Worker` interface with `work()` and `eat()` — a robot that works but doesn't eat is forced to implement `eat()`.  
**Fix:** Split into `Workable` and `Eatable`.

---

### D — Dependency Inversion Principle (DIP)
> *"High-level modules should not depend on low-level modules. Both should depend on abstractions."*

- Depend on **interfaces/abstractions**, not on **concrete classes**.
- Enables **loose coupling**.
- Related to **Dependency Injection** and **Inversion of Control (IoC)**.

**Example:** A `NotificationService` should depend on a `MessageSender` interface, not directly on `EmailSender` or `SMSSender` classes.

---

## L9 — Design Patterns & Singleton

### What Are Design Patterns?
- Reusable solutions to **common recurring design problems**.
- Not code — they are **guidelines/templates**.
- Overusing patterns leads to unnecessary complexity ("**patternitis**").
- Source: Gang of Four (GoF) book.

### Three Categories of Design Patterns

| Category | Deals With | Examples |
|---|---|---|
| **Creational** | Object creation | Singleton, Factory Method, Builder |
| **Structural** | Object composition | Adapter, Composite, Decorator |
| **Behavioral** | Object interaction & responsibility | Observer, Strategy, Command |

---

### Singleton Pattern (Creational)
**Purpose:** Ensure a class has **only one instance** and provides a **global access point** to it.

**Use cases:** Logging, configuration managers, database connections.

**Implementation:**
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}  // private constructor

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Key mechanics:**
- Constructor is **private** — prevents `new Singleton()` from outside.
- Static field holds the single instance.
- Static `getInstance()` method returns the cached instance.

**Pros:** Guaranteed single instance; lazy initialization; global access point.  
**Cons:** Violates SRP (solves two problems at once); hard to unit test; multithreading issues.

---

## L10 — Strategy Design Pattern (Behavioral)

**Purpose:** Define a family of algorithms, encapsulate each one, and make them interchangeable at runtime.

**Problem it solves:** A class with many conditional branches selecting different algorithms — hard to extend without modifying the class (violates OCP).

### Key Roles
- **Context:** The class that holds a reference to a `Strategy`. Delegates the work to the strategy.
- **Strategy (interface):** Declares the method all concrete strategies implement (e.g., `execute(data)`).
- **ConcreteStrategy:** Each specific algorithm implementation.
- **Client:** Creates a specific strategy and passes it to the context.

### Structure
```
Context → (has a) → Strategy (interface)
                        ↑
             ConcreteStrategyA, ConcreteStrategyB
```

**Example:** Navigation app with `RoadStrategy`, `WalkingStrategy`, `PublicTransportStrategy` — all implement `RouteStrategy`.

### Implementation Steps
1. Identify the algorithm prone to change.
2. Declare a `Strategy` interface with the method signature.
3. Extract each algorithm into its own class implementing the interface.
4. Add a strategy field + setter to the Context.
5. Client associates the context with the appropriate strategy.

**Pros:** Swap algorithms at runtime; follows OCP; replaces inheritance with composition.  
**Cons:** Overkill for only a few rarely-changing algorithms; clients must know the differences between strategies.

---

## L11 — Factory, Factory Method & Abstract Factory Patterns (Creational)

### Factory Pattern (Simple Factory)
- One **static method** that creates and returns objects of different subclasses based on input.
- The client calls the factory method instead of using `new` directly.
- Low flexibility — adding a new product requires modifying the factory class.

```java
public class ShapeFactory {
    public static Shape createShape(String type) {
        if (type.equals("circle")) return new Circle();
        if (type.equals("square")) return new Square();
        return null;
    }
}
```

---

### Factory Method Pattern
- Defines an interface/abstract method for creating objects, but **lets subclasses decide which class to instantiate**.
- Promotes **inheritance**; each subclass overrides the factory method.
- Follows **OCP** — add new products by adding new subclasses, not modifying existing code.
- Follows **SRP** — creation code is in one place.

**Structure:**
```
Creator (abstract)          Product (interface)
  + createProduct(): Product    + doStuff()
       ↑                              ↑
ConcreteCreatorA → returns ConcreteProductA
ConcreteCreatorB → returns ConcreteProductB
```

---

### Abstract Factory Pattern
- "**Factory of Factories**" — provides an interface for creating **families of related objects**.
- High flexibility; supports multiple product families.
- Uses **composition**, relies on **Dependency Inversion**.

---

### Comparison Table

| Feature | Factory (Simple) | Factory Method | Abstract Factory |
|---|---|---|---|
| Object creation | One static method | Subclass decides | Factory of factories |
| Flexibility | Low | Medium | High |
| Design principle | Encapsulation | OCP via inheritance | DIP via interfaces |
| Use case | Simple conditional creation | Delegate instantiation to subclasses | Independent product families |

---

## L12 — Adapter Design Pattern (Structural)

**Purpose:** Allow objects with **incompatible interfaces** to collaborate. Also called a **Wrapper**.

**Problem:** You have existing code expecting one interface, but a library/class you want to use has a different interface.

**Solution:** Create an Adapter class that:
1. Implements the interface your code expects (the **Target** interface).
2. Internally holds a reference to the incompatible object (the **Adaptee**).
3. Translates calls from the target interface into calls on the adaptee.

### Structure
```
Client → Target Interface ← Adapter → Adaptee
```

**Real-world analogy:** A power adapter that lets a US plug work in a European socket.

**Example:** A stock market app expecting JSON data adapts an XML-based analytics library using an XML-to-JSON adapter.

### How It Works
1. The adapter gets an interface compatible with the client.
2. The client safely calls the adapter's methods.
3. The adapter translates and forwards the request to the adaptee.
4. Two-way adapters are also possible.

**Pros:** Follows SRP (conversion logic is separate); follows OCP (new adapters without touching existing code).  
**Cons:** Increases overall complexity by adding new classes and interfaces.

---

## L13 — MVC Architecture

**MVC = Model – View – Controller**

**Purpose:** Separate an application into three components to improve **maintainability**, **scalability**, and **separation of concerns**.

### The Three Components

| Component | Responsibility | Examples |
|---|---|---|
| **Model** | Data and business logic; independent of UI | Data classes, business rules |
| **View** | Presentation layer; displays data to user | UI components, HTML pages |
| **Controller** | Mediates between Model and View; handles user input | Servlets, Spring MVC Controllers |

### Flow of Interaction
```
User → View → (sends request) → Controller → (manipulates) → Model
                ↑                                                |
                └──────────── (displays/renders) ───────────────┘
```

### Why MVC?
- Separation of concerns: each layer can change independently.
- Easier to test (can test Model without UI).
- Easier to maintain and scale.

### Considerations
- Requires careful design to avoid tight coupling between layers.
- Different frameworks implement MVC differently (Spring MVC, JavaFX, etc.).

---

## L14 — Observer Design Pattern (Behavioral)

**Purpose:** Define a **one-to-many dependency** so when one object changes state, all dependents are automatically notified and updated.

Also called: **Publish-Subscribe pattern**.

### Key Roles

| Role | Responsibility |
|---|---|
| **Subject (Publisher)** | Maintains a list of observers; provides attach/detach/notify methods |
| **Observer (Subscriber)** | Defines an `update()` interface called when subject changes |
| **ConcreteSubject** | Stores state; notifies observers on state change |
| **ConcreteObserver** | Implements `update()` to react to subject's changes |

### Structure
```java
// Subject interface
interface Subject {
    void attach(Observer o);
    void detach(Observer o);
    void notifyObservers();
}

// Observer interface
interface Observer {
    void update(float temperature);
}
```

### Example: Weather Station
```java
public class WeatherData implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private float temperature;

    public void registerObserver(Observer o) { observers.add(o); }
    public void removeObserver(Observer o) { observers.remove(o); }

    public void notifyObservers() {
        for (Observer o : observers) o.update(temperature);
    }

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        notifyObservers();
    }
}
```

### Observer + MVC Integration
- **Model** acts as the **Subject**.
- **Views** act as **Observers** — they listen for model changes and update automatically.
- **Controller** coordinates the interaction between Model and View.

**Benefits:** Loose coupling; supports broadcasting to multiple observers; enhances modularity.

---

## L15 — Exception Handling

### What Is an Exception?
An exception is an event that disrupts the normal flow of a program. Java uses exceptions to handle errors cleanly rather than crashing.

### Java Exception Hierarchy
```
Throwable
├── Error              (serious JVM problems — don't catch these)
└── Exception
    ├── IOException    (Checked — must handle or declare)
    ├── SQLException   (Checked)
    └── RuntimeException  (Unchecked — not checked at compile time)
        ├── NullPointerException
        ├── IllegalArgumentException
        └── ArrayIndexOutOfBoundsException
```

### Checked vs. Unchecked Exceptions

| | Checked | Unchecked |
|---|---|---|
| Checked at | Compile time | Runtime |
| Must handle? | Yes (catch or declare with `throws`) | No |
| Extends | `Exception` (not RuntimeException) | `RuntimeException` |
| Examples | `IOException`, `SQLException` | `NullPointerException`, `IllegalArgumentException` |

---

### try–catch–finally Syntax
```java
try {
    // Code that might throw an exception
} catch (IOException e) {
    // Handle IOException
} catch (Exception e) {
    // Handle any other exception
} finally {
    // Always runs — use for cleanup (close files, etc.)
}
```

- Multiple `catch` blocks: more specific exceptions first.
- `finally` always executes, even if an exception is thrown or caught.

---

### throw vs. throws

| Keyword | Usage |
|---|---|
| `throw` | Used inside a method to actually **throw** an exception: `throw new IllegalArgumentException("msg");` |
| `throws` | Used in method signature to **declare** that a method might throw a checked exception: `public void read() throws IOException` |

---

### Rethrowing Exceptions
```java
catch (IOException e) {
    throw new RuntimeException(e);  // Wrap checked as unchecked
}
```

---

### Custom Exceptions
```java
// Checked custom exception
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

// Unchecked custom exception
public class InvalidAgeException extends RuntimeException {
    public InvalidAgeException(String message) {
        super(message);
    }
}
```

---

### Best Practices & Fail-Fast Design

**Fail-fast principle:** Detect and report errors as early as possible.

- **Do throw exceptions** when: input is invalid, a precondition fails, an unexpected state is reached.
- **Don't catch** exceptions you can't meaningfully handle — let them propagate.
- **Don't use exceptions for control flow** (e.g., don't catch `NumberFormatException` to check if input is a number — validate first).
- Use **specific exception types** (don't just catch `Exception`).
- Always clean up resources in `finally` (or use **try-with-resources**).

---

## Quick Reference Summary

| Topic | Pattern/Principle | Type | Key Idea |
|---|---|---|---|
| SOLID – SRP | — | Principle | One class, one reason to change |
| SOLID – OCP | — | Principle | Open for extension, closed for modification |
| SOLID – LSP | — | Principle | Subtypes substitutable for base types |
| SOLID – ISP | — | Principle | Many small interfaces > one large interface |
| SOLID – DIP | — | Principle | Depend on abstractions, not concretions |
| Singleton | Creational | Pattern | One instance, global access point |
| Strategy | Behavioral | Pattern | Swappable algorithms via interface |
| Factory (Simple) | Creational | Pattern | Static creation method |
| Factory Method | Creational | Pattern | Subclass decides what to create |
| Abstract Factory | Creational | Pattern | Family of related objects |
| Adapter | Structural | Pattern | Bridge between incompatible interfaces |
| MVC | Architecture | Pattern | Model–View–Controller separation |
| Observer | Behavioral | Pattern | One-to-many state change notification |
| Exception Handling | — | Language Feature | try/catch/finally, checked vs unchecked |