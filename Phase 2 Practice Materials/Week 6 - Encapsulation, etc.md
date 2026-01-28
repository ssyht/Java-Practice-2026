# Week 6: Classes, Encapsulation, and Immutability

## Overview (From Your Professor's Slides)
- Designing good classes
- Encapsulation and access control
- Constructors and invariants
- Introduction to immutability

## What You'll Learn This Week
- **What is a class really?** - Represents a concept from the domain
- **Class responsibilities** - What does a class know, do, and not do?
- **Encapsulation** - Hide internal state, expose behavior
- **Access Modifiers** - public, private, protected, package-private
- **Constructors** - Initialize objects and enforce invariants
- **Class Invariants** - Rules that must always be true
- **The `this` keyword** - Referring to the current object
- **Validation** - Fail fast principle
- **Immutability** - Creating unchangeable objects
- **Getters and Setters** - When to use them (and when NOT to)

---

## Day 1: What Is a Class Really?

### Three Core Aspects (From Professor's Slides)

A class:
1. **Represents a concept from the domain**
2. **Bundles data and behavior**
3. **Protects its internal state**

### Class Responsibilities

When designing a class, ask these three questions:
1. **What does this class know?** (Data/State)
2. **What does this class do?** (Behavior/Methods)
3. **What does this class NOT do?** (Boundaries)

### Example Domain Concept: Task

**From the slides:**

```java
public class Task {
    public String description;
    public boolean completed;
}
```

**What does Task know?**
- Description (what the task is)
- Completion status (done or not done)

**What does Task do?**
- (Nothing yet - this is a problem!)

**What does Task NOT do?**
- Doesn't validate data
- Doesn't control changes
- This is an **anemic object** (zombie class - data without behavior)

---

## Day 2: Problems With Public Fields

### The Bad Example (From Slides)

```java
public class Task {
    public String description;
    public boolean completed;
}
```

### Problems With This Design:

**1. No Validation**
```java
Task task = new Task();
task.description = "";  // Empty description allowed!
task.description = null;  // Null allowed!
```

**2. No Control Over Changes**
```java
task.completed = true;
task.completed = false;
task.completed = true;  // Anyone can change anytime!
```

**3. Breaks Invariants**
- An **invariant** is a rule that must ALWAYS be true
- Example invariant: "description is never null or blank"
- With public fields, we can't enforce this!

**4. Hard to Refactor**
- If you want to change how `description` works internally, you break all existing code
- Everyone is directly accessing the field

---

## Day 3: Encapsulation

### What is Encapsulation? (From Slides)

**Three Principles:**
1. **Hide internal state** - Make fields private
2. **Expose behavior** - Provide public methods
3. **Control access through methods** - Validate and protect

### Visual Example

**Before Encapsulation (BAD):**
```java
public class Task {
    public String description;  // ❌ Anyone can access
    public boolean completed;   // ❌ Anyone can change
}

Task task = new Task();
task.description = "";  // Oops, empty!
task.completed = true;  // No validation!
```

**After Encapsulation (GOOD):**
```java
public class Task {
    private String description;  // ✅ Hidden/Protected
    private boolean completed;   // ✅ Hidden/Protected
    
    // Controlled access through methods
    public void markCompleted() {
        completed = true;
    }
}

Task task = new Task("Study Java");
task.markCompleted();  // ✅ Controlled behavior
// task.description = "";  // ❌ ERROR! Can't access private field
```

---

## Day 3-4: Access Modifiers in Java

### The Four Access Levels (From Slides)

| Modifier | Access Level |
|----------|-------------|
| `public` | Accessible everywhere |
| `private` | Accessible only within the class |
| `protected` | Accessible in subclasses and same package |
| (default) | Package-private - accessible within the package |

### Visibility Levels Explained

#### 1. **private** (Most Restrictive)
```java
public class Task {
    private String description;  // Only Task class can access
    
    private void validateDescription(String desc) {
        // Only methods inside Task can call this
    }
}
```

#### 2. **package-private** (No Modifier - Default)
```java
class Task {  // No public - package-private
    String description;  // No modifier - package-private
    
    void helper() {  // Package-private method
        // Accessible to classes in same package
    }
}
```

#### 3. **protected** (For Subclasses)
```java
public class Task {
    protected void validate() {
        // Accessible in subclasses and same package
    }
}
```

#### 4. **public** (Least Restrictive)
```java
public class Task {
    public void markCompleted() {  // Anyone can call
        // Public interface
    }
}
```

### Why private Is the Default Choice (From Slides)

**Three Reasons:**
1. **Protects invariants** - Rules stay enforced
2. **Reduces coupling** - Other code doesn't depend on internals
3. **Enables safe refactoring** - Can change internals without breaking other code

---

## Example: Field Visibility (From Slides)

```java
public class Task {
    private String description;
    private boolean completed;
}
```

**Key Point:** Fields should be `private` by default!

---

## Example: Method Visibility (From Slides)

```java
public class Task {
    // PUBLIC method - part of the interface
    public void markCompleted() {
        completed = true;
    }
    
    // PRIVATE method - internal helper
    private void validateDescription(String desc) {
        if (desc == null || desc.isBlank()) {
            throw new IllegalArgumentException();
        }
    }
}
```

**Key Insight:**
- **Public methods** = What the class DOES (behavior exposed to users)
- **Private methods** = HOW the class does it (internal implementation)

---

## Example: Package Private Visibility (From Slides)

```java
public class TaskService {
    public void addTask(Task task) {
        // package private - only classes in same package can call
    }
}
```

**When to use package-private:**
- Helper classes that work together
- Internal APIs not meant for external use
- Testing support methods

---

## Example: Protected Visibility (From Slides)

```java
protected void validate() {
    // for subclasses
}
```

**When to use protected:**
- Methods that subclasses need to override or access
- We'll learn more about this in Week 8 (Inheritance)

---

## Common Access Modifier Mistakes (From Slides)

### 1. Making Everything Public
```java
// ❌ BAD
public class Task {
    public String description;
    public boolean completed;
    public void helper1() { }
    public void helper2() { }
    public void helper3() { }
}
```

**Problem:** Everything is exposed! No encapsulation, no control.

### 2. Using Protected Instead of Composition
```java
// ❌ Often misused
protected String internalData;
```

**Problem:** Protected is for inheritance relationships, not a shortcut for package access.

### 3. Exposing Fields Directly
```java
// ❌ BAD
public class Task {
    public String description;  // Direct field access
}

// ✅ GOOD
public class Task {
    private String description;  // Hidden
    
    public String getDescription() {  // Controlled access
        return description;
    }
}
```

---

## Access Modifiers and Encapsulation (From Slides)

**Key Principles:**
1. **Access modifiers enforce boundaries** - They're the walls protecting your data
2. **Encapsulation is impossible without them** - Can't hide what's public
3. **Visibility reflects responsibility** - Public = "This is what I promise to do"

---

## Day 4-5: Adding Behavior (From Slides)

### The Better Version

```java
public class Task {
    private String description;
    private boolean completed;
    
    // Add behavior!
    public void markCompleted() {
        completed = true;
    }
}
```

**Key Principle:** Data and behavior belong together!

### Why This Is Better:

**Before (Anemic):**
```java
Task task = new Task();
task.completed = true;  // Just setting data
```

**After (Rich Object):**
```java
Task task = new Task("Study Java");
task.markCompleted();  // Meaningful behavior!
```

The method name `markCompleted()` tells you WHAT is happening, not just HOW.


---

## Day 5: Getter Methods (From Slides)

### Use Getters Carefully

**From professor:** "Getters are not evil, but should be used intentionally."

```java
public String getDescription() {
    return description;
}

public boolean isCompleted() {
    return completed;
}
```

**When to use getters:**
- When external code needs to READ the data
- For boolean: use `is` prefix (isCompleted, isActive, isValid)
- For other types: use `get` prefix (getDescription, getName)

**When NOT to use getters:**
- Don't automatically add getters for every field!
- Ask: "Does external code really need this?"
- Alternative: Provide behavior instead

### Example: Think About Behavior First

```java
// ❌ DON'T DO THIS
public double getBalance() {
    return balance;
}
// Then somewhere else: if (account.getBalance() > 100) { ... }

// ✅ BETTER: Provide behavior
public boolean hasMinimumBalance(double minimum) {
    return balance >= minimum;
}
// Use: if (account.hasMinimumBalance(100)) { ... }
```

---

## Day 5: Avoiding Setters by Default (From Slides)

### Why Setters Are Problematic

**From professor's slides:**
- Setters expose internal state
- They weaken encapsulation
- Prefer meaningful methods

### The Problem

```java
// ❌ BAD: Generic setter
public void setCompleted(boolean completed) {
    this.completed = completed;
}

// Usage:
task.setCompleted(true);  // What does this mean?
task.setCompleted(false); // Why is this false now?
```

### The Solution: Meaningful Method Names (From Slides)

```java
// ✅ GOOD: Meaningful methods
public void markCompleted() {
    completed = true;
}

public void markIncomplete() {
    completed = false;
}

public void rename(String newDescription) {
    validateDescription(newDescription);
    description = newDescription;
}

public void updateDescription(String newDescription) {
    validateDescription(newDescription);
    description = newDescription;
}
```

**Benefits:**
- Clear intention: `markCompleted()` vs `setCompleted(true)`
- Can add validation and business logic
- Makes code more readable

---

## Quick Check (From Slides)

**Which is better?**

```java
// Option 1
public void setBalance(double balance) {
    this.balance = balance;
}

// Option 2
public void deposit(double amount) {
    // validation and logic
}

public void withdraw(double amount) {
    // validation and logic
}
```

**Answer:** Option 2! 

**Why?**
- `deposit()` and `withdraw()` have clear meaning
- They can enforce rules (no negative deposits, no overdrafts)
- `setBalance()` is too generic and allows anything

---

## Encapsulation Benefits (From Slides)

### Three Key Benefits:

1. **Easier to change internals**
   - Change implementation without breaking external code
   - Example: Change from storing balance as cents to dollars

2. **Safer code**
   - Validation in one place
   - Invariants always enforced
   - No invalid states possible

3. **Clear responsibilities**
   - Each class has a well-defined interface
   - Easy to understand what class does
   - Easy to test

---

## Day 6-7: Constructors, Validation, and Immutability

### Lecture Goals (From Slides)
- Understand constructors
- Enforce invariants
- Learn immutability
- Introduce records

---

## What is a Class Invariant? (From Slides)

### Definition

**A class invariant is a condition that must hold for every valid instance of a class, before and after every public method call.**

**Important clarifications:**
- An invariant is NOT a field
- Fields store state
- Invariants constrain that state
- An invariant is a RULE about the state that must always be true

### Examples of Class Invariants (From Slides)

**For a Task class:**
- `description` is never null or blank
- `completed` is either true or false (trivial, but still a rule)

**For a BankAccount:**
- `balance` is never negative

**For a Person:**
- `age` is non-negative
- `name` is not empty

---

## What Is a Constructor? (From Slides)

### Three Purposes:

1. **Initializes object state** - Sets up the object
2. **Enforces invariants** - Makes sure rules are true from the start
3. **Runs exactly once** - When object is created with `new`

---

## Basic Constructor Example (From Slides)

```java
public class Task {
    private String description;
    private boolean completed;
    
    public Task(String description) {
        this.description = description;
        this.completed = false;
    }
}
```

**Using it:**
```java
Task task = new Task("Study Java");
// description is set
// completed is false
// Object is fully initialized!
```

---

## What is "this"? (From Slides)

### Four Uses of `this`:

1. **Refers to the current object**
   ```java
   public Task(String description) {
       this.description = description;
       // this.description = field
       // description = parameter
   }
   ```

2. **Disambiguates fields from parameters**
   ```java
   // Without this:
   public Task(String description) {
       description = description;  // ❌ Confusing! Which is which?
   }
   
   // With this:
   public Task(String description) {
       this.description = description;  // ✅ Clear!
   }
   ```

3. **Allows method chaining** (We'll see this later)
   ```java
   return this;
   ```

4. **Used to call another constructor** (Constructor chaining)
   ```java
   public Task(String description) {
       this(description, false);  // Calls other constructor
   }
   ```

---

## Validation in Constructors (From Slides)

### Follow the "Fail Fast" Principle!

**From professor:** "Fail fast means detecting invalid input or invalid state as early as possible and stopping execution immediately, usually by throwing an exception."

### Example from Slides:

```java
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException();
    }
    this.description = description;
}
```

**Why fail fast?**
- Catch errors immediately at creation time
- Don't let invalid objects exist
- Makes debugging easier (error happens where problem is)
- Protects invariants from the very beginning

### Better Validation Example:

```java
public class Task {
    private String description;
    private boolean completed;
    
    public Task(String description) {
        validateDescription(description);
        this.description = description;
        this.completed = false;
    }
    
    private void validateDescription(String desc) {
        if (desc == null || desc.isBlank()) {
            throw new IllegalArgumentException(
                "Description cannot be null or blank"
            );
        }
    }
    
    public void updateDescription(String newDescription) {
        validateDescription(newDescription);  // Reuse validation!
        this.description = newDescription;
    }
}
```

---

## Complete Encapsulated Class Example

```java
public class Task {
    // 1. Private fields (hide state)
    private String description;
    private boolean completed;
    
    // 2. Constructor (initialize and validate)
    public Task(String description) {
        if (description == null || description.isBlank()) {
            throw new IllegalArgumentException(
                "Description cannot be null or blank"
            );
        }
        this.description = description;
        this.completed = false;
    }
    
    // 3. Getters (careful, intentional access)
    public String getDescription() {
        return description;
    }
    
    public boolean isCompleted() {
        return completed;
    }
    
    // 4. Meaningful behavior (not generic setters)
    public void markCompleted() {
        completed = true;
    }
    
    public void rename(String newDescription) {
        if (newDescription == null || newDescription.isBlank()) {
            throw new IllegalArgumentException(
                "Description cannot be null or blank"
            );
        }
        this.description = newDescription;
    }
    
    // 5. Private helper methods
    private void validateDescription(String desc) {
        if (desc == null || desc.isBlank()) {
            throw new IllegalArgumentException(
                "Description cannot be null or blank"
            );
        }
    }
}
```

**This class demonstrates:**
- ✅ Encapsulation (private fields)
- ✅ Constructor with validation
- ✅ Invariants enforced (description never null/blank)
- ✅ Meaningful methods instead of setters
- ✅ Fail-fast validation
- ✅ Clear responsibilities

---

## Constructor Overloading


```java
public class Person {
    private String name;
    private int age;
    private String email;
    
    // Constructor 1: All parameters
    public Person(String name, int age, String email) {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name cannot be blank");
        }
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        this.name = name;
        this.age = age;
        this.email = email;
    }
    
    // Constructor 2: No email
    public Person(String name, int age) {
        this(name, age, "");  // Calls first constructor
    }
    
    // Constructor 3: Just name
    public Person(String name) {
        this(name, 0, "");  // Calls first constructor
    }
}
```

**Using them:**
```java
Person p1 = new Person("Alice", 25, "alice@email.com");
Person p2 = new Person("Bob", 30);
Person p3 = new Person("Charlie");
```

---

## Summary: Key Principles from This Week's Slides

### 1. What Is a Class?
- Represents a domain concept
- Bundles data and behavior
- Protects internal state

### 2. Class Responsibilities
- Know: What data does it have?
- Do: What behavior does it provide?
- NOT Do: What is outside its responsibility?

### 3. Encapsulation
- Hide internal state (private fields)
- Expose behavior (public methods)
- Control access through methods

### 4. Access Modifiers
- **private** - Default choice for fields
- **public** - For class interface
- **protected** - For subclasses (later)
- **(default)** - Package-private

### 5. Getters and Setters
- Use getters intentionally, not automatically
- Avoid setters - use meaningful methods instead
- Example: `markCompleted()` > `setCompleted(true)`

### 6. Constructors
- Initialize object state
- Enforce invariants
- Run exactly once
- Validate early (fail fast)

### 7. Class Invariants
- Rules that must ALWAYS be true
- Not fields - constraints on fields
- Examples: "balance never negative", "name never null"

### 8. The `this` Keyword
- Refers to current object
- Disambiguates fields from parameters
- Used for method chaining
- Used to call other constructors

---

## Week 6 Practice Exercises

### Exercise 1: BankAccount with Invariants

Create a `BankAccount` class that maintains these invariants:
- Account number is never null or empty
- Owner is never null or empty  
- Balance is never negative

**Requirements:**
```java
public class BankAccount {
    private String accountNumber;
    private String owner;
    private double balance;
    
    // Constructor - validate all parameters
    // deposit() - adds money (positive amounts only)
    // withdraw() - removes money (check sufficient balance)
    // getBalance() - returns balance
    // NO setBalance() method!
}
```

**Invariants to enforce:**
- `accountNumber` != null && !accountNumber.isBlank()
- `owner` != null && !owner.isBlank()
- `balance` >= 0

**Test it:**
```java
BankAccount acc = new BankAccount("12345", "Alice", 1000);
acc.deposit(500);
acc.withdraw(200);
// acc.withdraw(2000); // Should not allow overdraft!
```

### Exercise 2: Task Class (From Slides)

Create the complete `Task` class from your professor's slides:

**Requirements:**
```java
public class Task {
    private String description;
    private boolean completed;
    
    // Constructor with validation
    // getDescription()
    // isCompleted()
    // markCompleted()
    // rename(String newDescription) - with validation
}
```

**Invariant:** description is never null or blank

**Test it:**
```java
Task task = new Task("Study Java");
System.out.println(task.getDescription());
task.markCompleted();
System.out.println(task.isCompleted());
task.rename("Master Java");
```

### Exercise 3: Person with Age Validation

Create a `Person` class:

**Requirements:**
```java
public class Person {
    private String name;
    private int age;
    
    // Constructor with validation
    // getName()
    // getAge()
    // haveBirthday() - increments age
    // NO setAge() or setName()!
}
```

**Invariants:**
- name is not null or empty
- age >= 0

**Test it:**
```java
Person person = new Person("Alice", 25);
person.haveBirthday();
System.out.println(person.getAge()); // 26
```

### Exercise 4: Product with Price Validation

Create a `Product` class:

**Requirements:**
```java
public class Product {
    private String name;
    private double price;
    private int quantity;
    
    // Constructor with validation
    // getName()
    // getPrice()
    // getQuantity()
    // updatePrice(double newPrice) - validate positive
    // restock(int amount) - adds to quantity
    // sell(int amount) - reduces quantity if available
    // isInStock() - returns true if quantity > 0
}
```

**Invariants:**
- name is not null or empty
- price > 0
- quantity >= 0

### Exercise 5: Rectangle with Validation

Create an immutable `Rectangle` class:

**Requirements:**
```java
public class Rectangle {
    private final double width;   // Can't change after creation!
    private final double height;
    
    // Constructor with validation (positive dimensions)
    // getWidth()
    // getHeight()
    // getArea()
    // getPerimeter()
    // isSquare()
    // NO setters - this is immutable!
}
```

**Invariants:**
- width > 0
- height > 0

**Test it:**
```java
Rectangle rect = new Rectangle(5, 10);
System.out.println(rect.getArea()); // 50
System.out.println(rect.isSquare()); // false
// rect.width = 20; // ❌ ERROR - final field!
```

---

## Common Mistakes to Avoid

### 1. Making Everything Public
```java
// ❌ BAD
public class Task {
    public String description;
    public boolean completed;
}

// ✅ GOOD
public class Task {
    private String description;
    private boolean completed;
}
```

### 2. No Validation in Constructor
```java
// ❌ BAD
public Task(String description) {
    this.description = description;  // What if null?
}

// ✅ GOOD
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException();
    }
    this.description = description;
}
```

### 3. Using Generic Setters
```java
// ❌ BAD
public void setCompleted(boolean completed) {
    this.completed = completed;
}

// ✅ GOOD
public void markCompleted() {
    completed = true;
}
```

### 4. Forgetting `this` When Needed
```java
// ❌ CONFUSING
public Task(String description) {
    description = description;  // Which is which?
}

// ✅ CLEAR
public Task(String description) {
    this.description = description;
}
```

### 5. Not Enforcing Invariants
```java
// ❌ BAD - allows negative balance
public void withdraw(double amount) {
    balance -= amount;
}

// ✅ GOOD - maintains invariant
public void withdraw(double amount) {
    if (amount > 0 && balance >= amount) {
        balance -= amount;