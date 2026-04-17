# L11 – Factory, Factory Method & Abstract Factory Design Patterns
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe**

---

## The Problem (Motivation)

Imagine a logistics app. Version 1 only handles **Trucks** — so all the code is tightly coupled to `Truck`.

Now sea transportation companies want to be added. The result?
- Adding `Ship` requires **changing the entire codebase**
- Every new transport type means **doing it all again**
- You end up with nasty code full of `if/else` conditionals switching on transport type

> 🔑 The root issue: **object creation is scattered and tightly coupled** to concrete classes.

---

## The Three Factory Patterns (Overview)

| Pattern | Core Idea |
|---|---|
| **Factory (Simple)** | One static method that creates and returns objects based on a parameter |
| **Factory Method** | An abstract/overridable method in a creator class; subclasses decide what to create |
| **Abstract Factory** | An interface for creating *families* of related objects — a "factory of factories" |

---

## Pattern 1: Factory (Simple Factory)

### How It Works
- One class with a **single static method** that takes a parameter and returns the appropriate object
- The client calls the static method — no `new` keyword needed in client code
- All creation logic lives in **one place**

### Example

```java
// Product interface
interface Transport {
    void deliver();
}

// Concrete products
class Truck implements Transport {
    public void deliver() { System.out.println("Delivering by road"); }
}
class Ship implements Transport {
    public void deliver() { System.out.println("Delivering by sea"); }
}

// Simple Factory
class TransportFactory {
    public static Transport createTransport(String type) {
        if (type.equals("truck")) return new Truck();
        else if (type.equals("ship")) return new Ship();
        else throw new IllegalArgumentException("Unknown type");
    }
}

// Client
Transport t = TransportFactory.createTransport("truck");
t.deliver();
```

### Limitation
Adding a new transport type requires **modifying** `TransportFactory` → violates Open/Closed Principle.

---

## Pattern 2: Factory Method Pattern

### Core Idea
Replace the static creation method with an **abstract factory method** in a base Creator class. Each **subclass (Concrete Creator)** overrides it to return its own product type.

> "Define an interface for creating an object, but let subclasses decide which class to instantiate."

### The 4 Implementation Steps

**Step 1 — Define the Product interface**
```java
interface Transport {
    void deliver();
}
```

**Step 2 — Implement Concrete Products**
```java
class Truck implements Transport {
    public void deliver() { System.out.println("Delivering by road"); }
}
class Ship implements Transport {
    public void deliver() { System.out.println("Delivering by sea"); }
}
```

**Step 3 — Create the abstract Creator class**
```java
abstract class Logistics {
    // Factory method — subclasses must implement this
    public abstract Transport createTransport();

    // Business logic that uses the factory method
    public void planDelivery() {
        Transport t = createTransport();  // delegates creation
        t.deliver();
    }
}
```

**Step 4 — Override the factory method in Concrete Creators**
```java
class RoadLogistics extends Logistics {
    public Transport createTransport() {
        return new Truck();  // this subclass creates Trucks
    }
}

class SeaLogistics extends Logistics {
    public Transport createTransport() {
        return new Ship();   // this subclass creates Ships
    }
}
```

**Client usage:**
```java
Logistics logistics = new RoadLogistics();
logistics.planDelivery();  // uses Truck internally

logistics = new SeaLogistics();
logistics.planDelivery();  // uses Ship internally
```

### Key Insight
The `Logistics` base class **never calls `new Truck()` or `new Ship()` directly**. It only calls `createTransport()` — a method it doesn't fully know. Subclasses fill in the blank.

### Pros & Cons

| ✅ Pros | ❌ Cons |
|---|---|
| Avoids **tight coupling** between creator and concrete products | Can create many subclasses (100 product types = 100 creator subclasses) |
| **Single Responsibility Principle** — creation logic in one place | More complex than simple factory |
| **Open/Closed Principle** — add new types via new subclasses, no existing code changes | |

---

## Pattern 3: Abstract Factory Pattern

### Core Idea
Provide an **interface** for creating **families of related objects** — without specifying their concrete classes. Think of it as a factory of factories.

### When to Use
When your system needs to work with multiple **product families** (e.g., Windows UI components vs. Mac UI components), and you want to ensure products within a family are always used together.

### Structure Example

```java
// Abstract product interfaces
interface Button { void click(); }
interface Checkbox { void check(); }

// Windows family
class WindowsButton implements Button {
    public void click() { System.out.println("Windows button click"); }
}
class WindowsCheckbox implements Checkbox {
    public void check() { System.out.println("Windows checkbox check"); }
}

// Mac family
class MacButton implements Button {
    public void click() { System.out.println("Mac button click"); }
}
class MacCheckbox implements Checkbox {
    public void check() { System.out.println("Mac checkbox check"); }
}

// Abstract Factory interface
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete Factories
class WindowsFactory implements GUIFactory {
    public Button createButton() { return new WindowsButton(); }
    public Checkbox createCheckbox() { return new WindowsCheckbox(); }
}
class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}

// Client — only knows GUIFactory, not concrete classes
class Application {
    private Button button;
    private Checkbox checkbox;

    Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }
}
```

---

## Full Comparison Table

| Feature | Factory (Simple) | Factory Method | Abstract Factory |
|---|---|---|---|
| **Purpose** | Centralizes object creation | Subclasses decide what to instantiate | Creates families of related objects |
| **Flexibility** | Low — modifying factory required for new types | Medium — add new subclasses | High — supports multiple product families |
| **Pattern type** | Creational (simple variant) | Creational, uses **inheritance** | Creational, uses **composition** |
| **Client knows** | The factory method | The factory subclass | The factory interface |
| **Design principle** | Encapsulation of instantiation | Open/Closed (via inheritance) | Dependency Inversion (via factory interfaces) |
| **Key use case** | Simple conditional creation | Class delegates instantiation to subclasses | System independent of how products are created |

---

## If You Have 100 Product Types...

| Pattern | How many factory functions? |
|---|---|
| **Factory (Simple)** | 1 static function with conditionals |
| **Factory Method** | 100 subclasses, each with 1 overridden method |
| **Abstract Factory** | Multiple factory interfaces grouping related products |

> Which is better? It depends on the situation — simple factories are easier but less extensible; factory method is more flexible but creates more classes; abstract factory handles complex families but is the most complex to set up.

---

## Connecting to SOLID

| Pattern | SOLID Principle |
|---|---|
| Factory Method | **Open/Closed** — extend with new subclasses, don't modify existing code |
| Factory Method | **Single Responsibility** — creation logic separated from business logic |
| Abstract Factory | **Dependency Inversion** — client depends on abstract factory interface, not concrete classes |

---

## Key Vocabulary

| Term | Definition |
|---|---|
| **Product** | The object being created (e.g., `Transport`, `Button`) |
| **Concrete Product** | A specific implementation of the product interface (e.g., `Truck`, `Ship`) |
| **Creator** | The abstract class declaring the factory method |
| **Concrete Creator** | Subclass that overrides the factory method to return a specific product |
| **Factory Method** | An overridable method in the Creator that returns a Product |
| **Abstract Factory** | An interface that groups multiple factory methods for a product family |
| **Product Family** | A set of related products designed to be used together (e.g., Windows UI widgets) |
| **Tight Coupling** | When one class directly depends on another concrete class — what factories help avoid |