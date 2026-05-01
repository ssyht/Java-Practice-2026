# L14 – Observer Design Pattern
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe**

---

## What Is the Observer Pattern?

The **Observer** is a **Behavioral design pattern** — meaning it deals with how objects *communicate* and *interact* with each other.

Its formal definition: it defines a **one-to-many dependency** between objects so that when one object changes state, **all its dependents are notified and updated automatically**.

In plain terms: one object is "in charge" and many others are "watching" it. When something changes, everyone who's watching gets told instantly — without anyone having to constantly ask.

---

## The Problem It Solves

### Scenario: Customer Waiting for a New iPhone

Imagine you have a **Customer** who wants to buy the new iPhone when it arrives at a **Store**.

**Option 1 — Customer keeps checking (polling):**
- The customer visits the store every single day to see if the iPhone is in stock.
- Most trips are pointless because the product hasn't arrived yet.
- Wasteful for the customer.

**Option 2 — Store emails everyone:**
- The store blasts an email to every customer whenever *any* product arrives.
- Customers who don't care get spammed.
- Wasteful for the store and annoying to uninterested customers.

**The conflict:** Either the customer wastes time polling, or the store wastes resources notifying people who don't care.

**The Observer pattern solves this** — customers who *want* to know can *subscribe* to notifications. Only they get notified, and only when something actually changes.

---

## The Solution: Publisher + Subscribers

The lecture introduces two key terms:

- **Publisher (Subject)** — the object that *has* interesting state and *sends* notifications. (The store)
- **Subscribers (Observers)** — objects that *want to know* when something changes and *sign up* to receive notifications. (Interested customers)

The mechanism is simple — two things inside the publisher:
1. A **list** that stores references to all current subscribers
2. **Public methods** to `subscribe` (add) and `unsubscribe` (remove) from that list

When the publisher's state changes, it loops through the list and calls `update()` on every subscriber.

---

## Key Participants

| Participant | Role |
|---|---|
| **Subject** (interface) | Declares `attach()`, `detach()`, and `notifyObservers()` |
| **Concrete Subject** | Implements Subject; holds the actual state; calls `notifyObservers()` when state changes |
| **Observer** (interface) | Declares the `update()` method |
| **Concrete Observer** | Implements Observer; reacts when `update()` is called |

---

## UML Structure

```
[ Subject (interface) ]
  + attach(observer)
  + detach(observer)
  + notifyObservers()
        ▲
        │ implements
        │
[ WeatherData ]                     [ Observer (interface) ]
  - observers: List<Observer>          + update(temperature)
  - temperature: float                       ▲
  + registerObserver(o)                      │ implements
  + removeObserver(o)                        │
  + notifyObservers()               [ CurrentConditionsDisplay ]
  + setTemperature(temp)
```

`WeatherData` is the Subject. `CurrentConditionsDisplay` is the Observer. When `setTemperature()` is called, it triggers `notifyObservers()`, which calls `update()` on every registered observer.

---

## Java Code Walkthrough: Weather Station

### Step 1 — The Observer Interface

```java
public interface Observer {
    void update(float temperature);
}
```

Every class that wants to be notified must implement this interface. When the Subject has new data, it calls `update()` on each Observer.

---

### Step 2 — The Subject Interface

```java
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```

Any class that wants to *publish* events must implement this. It manages the subscriber list and broadcasts changes.

---

### Step 3 — The Concrete Subject: `WeatherData`

```java
import java.util.ArrayList;
import java.util.List;

public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;

    public WeatherData() {
        observers = new ArrayList<>();
    }

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature); // push new data to each observer
        }
    }

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        notifyObservers(); // automatically notify everyone when temp changes
    }
}
```

**Key moment:** `setTemperature()` stores the new value and immediately calls `notifyObservers()`. No observer has to ask — they get told.

---

### Step 4 — The Concrete Observer: `CurrentConditionsDisplay`

```java
public class CurrentConditionsDisplay implements Observer {
    private float temperature;

    public void update(float temperature) {
        this.temperature = temperature;
        display();
    }

    public void display() {
        System.out.println("Current temperature: " + temperature + "F");
    }
}
```

When `update()` is called, it stores the new temperature and prints it. Simple and focused — it only cares about displaying data.

---

### Step 5 — Wiring It All Together

```java
public class WeatherStation {
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();
        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay();

        weatherData.registerObserver(currentDisplay); // subscribe

        weatherData.setTemperature(80); // triggers update → prints "Current temperature: 80.0F"
    }
}
```

You could register multiple displays — a humidity display, a forecast display, etc. — and all of them would automatically receive updates whenever `setTemperature()` is called.

---

## Benefits

**Loose coupling** — The Subject and Observer only know about each other through interfaces. `WeatherData` doesn't know or care what `CurrentConditionsDisplay` does internally.

**Broadcasting** — One change notifies *all* registered observers simultaneously.

**Modularity & Reusability** — Observers can be added, removed, or swapped without touching the Subject at all.

---

## Considerations (Watch Out For)

**Memory leaks** — If observers aren't properly removed (unsubscribed), the Subject holds references to them indefinitely, preventing garbage collection.

**Push vs. Pull model** — The example above is a *push* model (Subject sends data directly via `update(temperature)`). A *pull* model would pass `this` (the Subject itself) in `update()` and let the Observer fetch whatever data it needs. Both are valid.

---

## Observer + MVC Together

This is the big connection the lecture makes — **MVC naturally uses the Observer pattern**:

| MVC Role | Observer Role |
|---|---|
| **Model** | Subject (publisher) |
| **View** | Observer (subscriber) |
| **Controller** | Wires them together and triggers changes |

When the Model's data changes, it notifies all registered Views automatically. The Views re-render. The Controller coordinates but doesn't need to manually update each View.

---

## MVC + Observer: Weather App Example

### Model (`WeatherDataModel`) — extends `Observable`

```java
import java.util.Observable;

public class WeatherDataModel extends Observable {
    private float temperature;

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        setChanged();            // mark that state has changed
        notifyObservers(temperature); // notify all registered observers
    }
}
```

`Observable` is a built-in Java class that handles the subscriber list for you. `setChanged()` must be called before `notifyObservers()` or nothing happens.

---

### View (`WeatherDisplayView`) — implements `Observer`

```java
import java.util.Observer;
import java.util.Observable;

public class WeatherDisplayView implements Observer {
    public void update(Observable o, Object arg) {
        if (o instanceof WeatherDataModel) {
            float temperature = (Float) arg;
            displayTemperature(temperature);
        }
    }

    private void displayTemperature(float temperature) {
        System.out.println("Current temperature: " + temperature + "F");
    }
}
```

The View implements `Observer`. It receives the notification and updates the display — no polling, no manual triggering from the Controller.

---

### Controller (`WeatherAppController`)

```java
public class WeatherAppController {
    private WeatherDataModel model;
    private WeatherDisplayView view;

    public WeatherAppController() {
        model = new WeatherDataModel();
        view = new WeatherDisplayView();
        model.addObserver(view); // wire the View as an Observer of the Model
    }

    public void setTemperature(float temperature) {
        model.setTemperature(temperature); // triggers automatic View update
    }
}
```

The Controller wires everything together at startup and delegates actions to the Model. It does NOT manually tell the View to update — the Observer pattern handles that automatically.

---

### Main

```java
public class WeatherApp {
    public static void main(String[] args) {
        WeatherAppController controller = new WeatherAppController();
        controller.setTemperature(80); // → Model updates → View auto-refreshes
    }
}
```

---

## Pattern Comparison: All Three Patterns So Far

| | Adapter (L12) | MVC (L13) | Observer (L14) |
|---|---|---|---|
| **Type** | Structural | Architectural | Behavioral |
| **Goal** | Bridge incompatible interfaces | Separate data, UI, and control | Notify dependents of state changes |
| **Key idea** | Wrap & translate | Organize into 3 roles | Subscribe & broadcast |
| **Relationship** | Class-to-class | App-wide structure | One-to-many |
| **SOLID link** | SRP, OCP | SRP | OCP, loose coupling |

---

## Key Takeaways

| Concept | Summary |
|---|---|
| **Pattern type** | Behavioral |
| **Core relationship** | One Subject → many Observers |
| **Subject does** | Maintains subscriber list; notifies on change |
| **Observer does** | Implements `update()`; reacts to notifications |
| **Main benefit** | Loose coupling — neither side needs to know details of the other |
| **In MVC** | Model = Subject, View = Observer, Controller = wires them |
| **Watch out for** | Memory leaks (unsubscribed observers), push vs. pull tradeoffs |