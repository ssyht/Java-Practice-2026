# L12 – Adapter Design Pattern
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe**

---

## What Is the Adapter Design Pattern?

The **Adapter** is a **Structural design pattern** — meaning it deals with how classes and objects are *composed* and *structured* together.

Its job: allow two objects with **incompatible interfaces** to work together without changing either of them.

It is also commonly called a **Wrapper**, because it wraps one object to make it look like something else.

> Think of a real-world power adapter: your laptop charger works in the US (Type A plug), but in Europe the outlets are different (Type C). You don't rewire your laptop or the building — you use an **adapter** between them.

---

## The Problem It Solves

### Scenario: Stock Market Monitoring App

Imagine you're building a stock market app:

- Your app **downloads stock data in XML format** from various providers.
- You want to add a **smart 3rd-party analytics library** to generate charts and insights.
- Problem: **the analytics library only accepts data in JSON format**.

```
Stock Data Provider → [XML] → Your App → [XML] ✗→ [JSON] Analytics Library
                                                  ↑ INCOMPATIBLE!
```

### Why Not Just Fix the Library?

You might think: *"Just modify the analytics library to accept XML!"* But:

1. **It might break other code** that already relies on the library working with JSON.
2. **You might not have access to the library's source code** — it's a third-party tool you just downloaded.

So modifying the library is often **impossible** or **too risky**.

---

## The Solution: Create an Adapter

An **Adapter** is a special class that:
1. **Accepts the format your code produces** (XML, in this case)
2. **Converts it internally** to what the other system expects (JSON)
3. **Passes the converted data** to the target system

The target system (the analytics library) never even knows an adapter is involved — it just receives the JSON it expects.

```
Stock Data Provider → [XML] → Your App → [XML] → Adapter → [JSON] → Analytics Library
                                                   ↑ Translates behind the scenes!
```

---

## How the Adapter Works (Step by Step)

1. The **adapter implements an interface** that your existing code already knows how to use.
2. Your code calls the adapter's methods **as if it were the real target**.
3. The adapter **converts** the request into the format the target expects.
4. The adapter **calls the target** with the correctly formatted data.

> The wrapped object is never aware of the adapter. It just receives requests in its expected format.

Sometimes you can even build a **two-way adapter** that translates in both directions.

---

## Real-Life Analogies

| Real World | Programming Equivalent |
|---|---|
| Power plug adapter (US ↔ EU) | Adapter class between two interfaces |
| Headphone jack adapter (3.5mm ↔ USB-C) | Converting one data format to another |
| Travel adapter kit | A set of adapters for different regions/contexts |

---

## UML Structure (Conceptual)

```
Client Code
    |
    ▼
[ Target Interface ]    ← What your code expects
    |
    ▼
[ Adapter Class ]       ← Implements Target, wraps Adaptee
    |
    ▼
[ Adaptee (e.g., JSON Analytics Library) ]   ← What you're trying to use
```

- **Client** – your existing code that can't be changed
- **Target Interface** – the interface your client knows
- **Adapter** – bridges the gap; translates calls
- **Adaptee** – the incompatible class you want to use

---

## Java Code Example

```java
// The interface your app already uses (XML-based)
interface XmlDataProcessor {
    void processXml(String xmlData);
}

// Third-party analytics library that only works with JSON
class AnalyticsLibrary {
    public void analyzeJson(String jsonData) {
        System.out.println("Analyzing JSON: " + jsonData);
    }
}

// The ADAPTER: wraps AnalyticsLibrary, accepts XML, converts to JSON
class XmlToJsonAdapter implements XmlDataProcessor {
    private AnalyticsLibrary analyticsLibrary;

    public XmlToJsonAdapter(AnalyticsLibrary library) {
        this.analyticsLibrary = library;
    }

    @Override
    public void processXml(String xmlData) {
        // Convert XML to JSON (simplified)
        String jsonData = convertXmlToJson(xmlData);
        analyticsLibrary.analyzeJson(jsonData);
    }

    private String convertXmlToJson(String xml) {
        // Conversion logic here
        return "{ \"data\": \"converted from XML\" }";
    }
}

// Client code — works with XmlDataProcessor, unaware of the library
public class StockApp {
    public static void main(String[] args) {
        AnalyticsLibrary library = new AnalyticsLibrary();
        XmlDataProcessor adapter = new XmlToJsonAdapter(library);

        adapter.processXml("<stock><price>100</price></stock>");
        // Output: Analyzing JSON: { "data": "converted from XML" }
    }
}
```

**Key idea:** `StockApp` only ever calls `processXml()` — it never interacts with `AnalyticsLibrary` directly. The adapter handles all the conversion under the hood.

---

## Pros and Cons

### ✅ Pros

**Single Responsibility Principle (SRP)**
- The conversion/adaptation logic lives in its own class.
- Your core business logic stays clean and doesn't get polluted with format conversion code.

**Open/Closed Principle (OCP)**
- You can introduce new adapters without modifying existing client code.
- Your app stays open for extension (new adapters) but closed for modification (don't touch working code).

---

### ❌ Cons

**Increased Complexity**
- You're adding new classes and interfaces to your codebase.
- If the incompatibility is minor, it might be simpler to just update the service class directly rather than build a whole adapter.

---

## When Should You Use the Adapter Pattern?

Use it when:
- You want to use an **existing class** but its interface doesn't match what you need.
- You're integrating a **third-party library** you can't modify.
- You want **legacy code** to work with a newer system without rewriting everything.
- You need **reusable classes** that cooperate with classes whose interfaces aren't yet known.

---

## Key Takeaways

| Concept | Summary |
|---|---|
| Pattern type | Structural |
| Also called | Wrapper |
| Core idea | Translate one interface into another |
| Main participants | Client, Target Interface, Adapter, Adaptee |
| SOLID principles | Supports SRP and OCP |
| When to use | Incompatible interfaces, third-party libraries, legacy integration |