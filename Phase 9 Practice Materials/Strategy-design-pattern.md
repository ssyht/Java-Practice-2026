# L10 – Strategy Design Pattern
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe**

---

## The Problem (Motivation)

Imagine a navigation app. You start with **road routing**, then add walking routes, then public transit, then cycling, then tourist attraction routes...

**Every new routing algorithm bloats the main `Navigator` class.**

Problems that arise:
- The class **doubles in size** with every new algorithm added
- A bug fix in one algorithm risks breaking **already-working code**
- The class becomes **too hard to maintain**

> 🔑 The root issue: **behavior that varies** is tangled inside a class that shouldn't own it.

---

## The Solution — Strategy Pattern

**Extract** each algorithm variant into its own separate class (a **strategy**).  
The original class (the **context**) holds a *reference* to a strategy and delegates work to it.

### Key Ideas
- The context **doesn't pick** the algorithm — the **client** does
- The context works with all strategies through a **common interface**
- You can **swap strategies at runtime** without touching the context
- Adding a new algorithm = adding a new class, **no changes to existing code**

---

## Structure

```
        ┌──────────────────┐         «interface»
        │    Context       │◇────────►  Strategy
        │------------------|          ├──────────────┤
        │ - strategy       │          │ + execute()  │
        │ + setStrategy()  │          └──────────────┘
        │ + doSomething()  │                  △
        └──────────────────┘          ┌───────┴────────┐
               ▲                      │                │
           Client           ConcreteStrategyA   ConcreteStrategyB
                             + execute()         + execute()
```

### The 4 Players

| Role | Description |
|---|---|
| **Strategy (interface)** | Declares the method all algorithms must implement (e.g., `execute()`) |
| **ConcreteStrategy** | A specific algorithm that implements the Strategy interface |
| **Context** | Holds a reference to a Strategy; delegates to it via the interface |
| **Client** | Creates a concrete strategy and passes it to the context |

---

## UML (Navigator Example)

```
Navigator                    «interface»
─────────────           ─────────────────────
- routeStrategy  ◇───►   RouteStrategy
+ buildRoute(A,B)        + buildRoute(A,B)
                                  △
                    ┌─────────────┼──────────────┐
                    │             │              │
              RoadStrategy  WalkingStrategy  PublicTransportStrategy
```

---

## Implementation Steps

1. **Identify** the algorithm in the context class that changes frequently (often a big `if/else` or `switch`)
2. **Declare** a Strategy interface (or abstract class) with a single method common to all variants
3. **Extract** each algorithm variant into its own class implementing the interface
4. **Add a field** in the context to store a Strategy reference; add a setter to change it
5. **Client** creates the desired concrete strategy and passes it to the context

---

## Java Code Example

```java
// Step 2: Strategy Interface
interface RouteStrategy {
    void buildRoute(String pointA, String pointB);
}

// Step 3: Concrete Strategies
class RoadStrategy implements RouteStrategy {
    public void buildRoute(String a, String b) {
        System.out.println("Building road route from " + a + " to " + b);
    }
}

class WalkingStrategy implements RouteStrategy {
    public void buildRoute(String a, String b) {
        System.out.println("Building walking route from " + a + " to " + b);
    }
}

class PublicTransportStrategy implements RouteStrategy {
    public void buildRoute(String a, String b) {
        System.out.println("Building transit route from " + a + " to " + b);
    }
}

// Step 4: Context
class Navigator {
    private RouteStrategy routeStrategy;  // holds a reference

    public void setRouteStrategy(RouteStrategy strategy) {
        this.routeStrategy = strategy;    // swap at runtime!
    }

    public void buildRoute(String a, String b) {
        routeStrategy.buildRoute(a, b);   // delegates to strategy
    }
}

// Step 5: Client
public class Main {
    public static void main(String[] args) {
        Navigator nav = new Navigator();

        nav.setRouteStrategy(new RoadStrategy());
        nav.buildRoute("Home", "Work");   // road route

        nav.setRouteStrategy(new WalkingStrategy());
        nav.buildRoute("Hotel", "Museum"); // walking route
    }
}
```

---

## Pros & Cons

| ✅ Pros | ❌ Cons |
|---|---|
| Swap algorithms **at runtime** | Overkill if only a couple algorithms exist and rarely change |
| Isolates algorithm details from the code that uses it | Clients must **know the difference** between strategies to choose one |
| Replace inheritance with **composition** | Modern languages support lambdas, so simple cases don't need full classes |
| Follows **Open/Closed Principle** — add new strategies without changing the context | |

---

## Strategy vs. Singleton — Quick Comparison

| | Singleton | Strategy |
|---|---|---|
| **Category** | Creational | Behavioral |
| **Purpose** | Control object creation | Encapsulate interchangeable algorithms |
| **Key mechanism** | Private constructor + static method | Interface + composition |
| **Runtime flexibility** | No (one fixed instance) | Yes (swap strategies freely) |

---

## Design Principle Connection

Strategy directly supports the **Open/Closed Principle (OCP)** from SOLID:
> *"Open for extension, closed for modification."*

Adding a new routing mode = write a new class. The `Navigator` context never changes.

---

## Key Vocabulary

| Term | Definition |
|---|---|
| **Strategy Pattern** | Behavioral pattern that encapsulates algorithms into interchangeable classes |
| **Context** | The class that holds and delegates to a strategy |
| **Strategy (interface)** | Common contract all concrete strategies implement |
| **ConcreteStrategy** | One specific implementation of the algorithm |
| **Composition** | Holding a reference to another object (as opposed to inheriting from it) |
| **Delegation** | When an object hands off work to another object |
| **Open/Closed Principle** | Extend behavior by adding new classes, not by modifying existing ones |