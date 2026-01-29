# Quiz Preparation Guide - Week 6
## Classes, Encapsulation, and Immutability

Based on your professor's lectures:
- L1: Java and Object Oriented Foundations
- L2: Classes, Encapsulation, and Immutability

---

## Key Topics You MUST Know

### 1. What Is a Class?
- **Definition:** Represents a concept from the domain
- **Three Core Aspects:**
  1. Bundles data and behavior
  2. Protects its internal state
  3. Represents a real-world concept

**Quiz Question Example:**
Q: What are the three things a class does?
A: Represents a domain concept, bundles data and behavior, protects internal state

---

### 2. Class Responsibilities

**Three Questions to Ask:**
1. **What does this class know?** (Data/Fields)
2. **What does this class do?** (Behavior/Methods)
3. **What does this class NOT do?** (Boundaries/Limits)

**Quiz Question Example:**
Q: When designing a class, what three responsibility questions should you ask?
A: What does it know? What does it do? What does it NOT do?

---

### 3. Problems With Public Fields

**Know These Problems:**
1. ❌ No validation
2. ❌ No control over changes
3. ❌ Breaks invariants
4. ❌ Hard to refactor

**Bad Example (Know This!):**
```java
public class Task {
    public String description;  // ❌ BAD
    public boolean completed;   // ❌ BAD
}
```

**Quiz Question Example:**
Q: What are the problems with making fields public?
A: No validation, no control over changes, breaks invariants, hard to refactor

---

### 4. Encapsulation

**Definition:** 
- Hide internal state
- Expose behavior
- Control access through methods

**The Three Principles:**
1. **Hide** internal state (private fields)
2. **Expose** behavior (public methods)
3. **Control** access through methods

**Good Example:**
```java
public class Task {
    private String description;  // ✅ Hidden
    private boolean completed;   // ✅ Protected
    
    public void markCompleted() {  // ✅ Controlled behavior
        completed = true;
    }
}
```

**Quiz Question Example:**
Q: What are the three principles of encapsulation?
A: Hide internal state, expose behavior, control access through methods

---

### 5. Access Modifiers

**You MUST memorize this table:**

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|------------|--------------|----------|------------|
| `private` | ✅ | ❌ | ❌ | ❌ |
| (default) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

**Visibility Levels:**
- **public:** accessible everywhere
- **private:** accessible only within the class
- **protected:** accessible in subclasses and same package
- **package-private (default):** accessible within the package

**Quiz Question Example:**
Q: Which access modifier allows access in subclasses but not everywhere?
A: protected

Q: What's the most restrictive access modifier?
A: private

---

### 6. Why private Is the Default Choice

**Three Reasons (From Slides):**
1. **Protects invariants**
2. **Reduces coupling**
3. **Enables safe refactoring**

**Quiz Question Example:**
Q: Why should fields be private by default?
A: Protects invariants, reduces coupling, enables safe refactoring

---

### 7. Common Access Modifier Mistakes

**Three Common Mistakes:**
1. ❌ Making everything public
2. ❌ Using protected instead of composition
3. ❌ Exposing fields directly

**Quiz Question Example:**
Q: Name three common access modifier mistakes.
A: Making everything public, using protected instead of composition, exposing fields directly

---

### 8. Getters and Setters

**Key Points:**
- **Getters:** "Not evil, but should be used intentionally"
- Use `get` prefix: `getDescription()`
- Use `is` prefix for boolean: `isCompleted()`

**Avoid Setters:**
- Setters expose internal state
- They weaken encapsulation
- **Prefer meaningful methods**

**Examples:**
```java
// ✅ GOOD: Getter
public String getDescription() {
    return description;
}

public boolean isCompleted() {
    return completed;
}

// ❌ BAD: Generic setter
public void setCompleted(boolean completed) {
    this.completed = completed;
}

// ✅ GOOD: Meaningful method
public void markCompleted() {
    completed = true;
}
```

**Quiz Question Example:**
Q: Why should you avoid generic setters?
A: They expose internal state and weaken encapsulation. Use meaningful methods instead.

Q: What's better: `setCompleted(true)` or `markCompleted()`?
A: `markCompleted()` - more meaningful and intention-revealing

---

### 9. Meaningful Method Names

**Examples from Slides:**
- ✅ `markCompleted` (not setCompleted)
- ✅ `rename` (not setDescription)
- ✅ `updateDescription` (not setDescription)
- ✅ `deposit(amount)` (not setBalance)
- ✅ `withdraw(amount)` (not setBalance)

**Quiz Question Example:**
Q: Which is better and why: `setBalance(double)` or `deposit(double)` and `withdraw(double)`?
A: `deposit()` and `withdraw()` are better because they have clear meaning, can enforce rules, and are more intention-revealing

---

### 10. Encapsulation Benefits

**Three Key Benefits:**
1. **Easier to change internals**
2. **Safer code**
3. **Clear responsibilities**

**Quiz Question Example:**
Q: What are the three benefits of encapsulation?
A: Easier to change internals, safer code, clear responsibilities

---

### 11. Class Invariants

**CRITICAL DEFINITION:**
"A class invariant is a condition that must hold for every valid instance of a class, before and after every public method call."

**Key Points:**
- An invariant is NOT a field
- Fields store state
- Invariants constrain that state
- An invariant is a RULE about the state

**Examples to Know:**
- **Task:** description is never null or blank
- **Task:** completed is either true or false
- **BankAccount:** balance is never negative
- **Person:** age is non-negative
- **Person:** name is not empty

**Quiz Question Example:**
Q: What is a class invariant?
A: A condition that must hold for every valid instance of a class, before and after every public method call. It's a rule about the state, not a field.

Q: Give an example of an invariant for a BankAccount class.
A: Balance is never negative

---

### 12. Constructors

**What Is a Constructor?**
- Initializes object state
- Enforces invariants
- Runs exactly once

**Constructor Syntax:**
```java
public ClassName(parameters) {
    // initialization code
}
```

**Example:**
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

**Quiz Question Example:**
Q: What are the three purposes of a constructor?
A: Initialize object state, enforce invariants, runs exactly once

Q: When does a constructor run?
A: Exactly once, when the object is created with `new`

---

### 13. The `this` Keyword

**Four Uses:**
1. **Refers to the current object**
2. **Disambiguates fields from parameters**
3. **Allows method chaining**
4. **Calls another constructor**

**Examples:**
```java
// Use 1 & 2: Disambiguate
public Task(String description) {
    this.description = description;  // this.description = field
}

// Use 4: Constructor chaining
public Person(String name) {
    this(name, 0, "");  // Calls another constructor
}
```

**Quiz Question Example:**
Q: What are the four uses of the `this` keyword?
A: Refers to current object, disambiguates fields from parameters, allows method chaining, calls another constructor

Q: What does `this.description = description` mean?
A: `this.description` is the instance field, `description` is the parameter

---

### 14. Validation in Constructors

**Fail Fast Principle:**
"Detecting invalid input or invalid state as early as possible and stopping execution immediately, usually by throwing an exception."

**Example:**
```java
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException();
    }
    this.description = description;
}
```

**Why Fail Fast?**
- Catch errors immediately at creation time
- Don't let invalid objects exist
- Makes debugging easier
- Protects invariants from the beginning

**Quiz Question Example:**
Q: What does "fail fast" mean?
A: Detecting invalid input or state as early as possible and stopping execution immediately by throwing an exception

Q: Why should you validate in constructors?
A: To enforce invariants from the start, catch errors early, and prevent invalid objects from existing

---

### 15. Access Modifiers and Encapsulation

**Key Relationship:**
- Access modifiers enforce boundaries
- Encapsulation is impossible without them
- Visibility reflects responsibility

**Quiz Question Example:**
Q: Why are access modifiers necessary for encapsulation?
A: Access modifiers enforce boundaries. Encapsulation is impossible without them. Visibility reflects responsibility.

---

### 16. Procedural vs OOP Thinking

**Procedural:**
- Focus on steps (what to do)
- Series of functions

**Object-Oriented:**
- Focus on responsibilities (who does what)
- Objects collaborate

**Quiz Question Example:**
Q: What's the difference between procedural and object-oriented thinking?
A: Procedural focuses on steps (what to do), OOP focuses on responsibilities (who does what)

---

### 17. Package-Private Visibility

**Definition:** No modifier = package-private

**Example:**
```java
public class TaskService {
    void addTask(Task task) {  // package-private
        // only classes in same package can call
    }
}
```

**Quiz Question Example:**
Q: What does it mean when a method has no access modifier?
A: It's package-private (default) - accessible only within the same package

---

### 18. Protected Visibility

**Definition:** Accessible in subclasses and same package

**Example:**
```java
protected void validate() {
    // for subclasses
}
```

**Quiz Question Example:**
Q: When should you use protected?
A: For methods that subclasses need to access or override

---

## Code Examples You Should Be Able to Write

### Example 1: Complete Task Class

```java
public class Task {
    private String description;
    private boolean completed;
    
    // Constructor with validation
    public Task(String description) {
        if (description == null || description.isBlank()) {
            throw new IllegalArgumentException(
                "Description cannot be null or blank"
            );
        }
        this.description = description;
        this.completed = false;
    }
    
    // Getters
    public String getDescription() {
        return description;
    }
    
    public boolean isCompleted() {
        return completed;
    }
    
    // Meaningful behavior (not generic setter)
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
}
```

**What This Demonstrates:**
- ✅ Private fields (encapsulation)
- ✅ Constructor with validation (fail fast)
- ✅ Invariant enforced (description never null/blank)
- ✅ Getters (intentional access)
- ✅ Meaningful methods (not setters)
- ✅ Uses `this` keyword correctly

---

### Example 2: BankAccount

```java
public class BankAccount {
    private String accountNumber;
    private String owner;
    private double balance;
    
    // Constructor with validation
    public BankAccount(String accountNumber, String owner, double balance) {
        if (accountNumber == null || accountNumber.isBlank()) {
            throw new IllegalArgumentException("Account number required");
        }
        if (owner == null || owner.isBlank()) {
            throw new IllegalArgumentException("Owner required");
        }
        if (balance < 0) {
            throw new IllegalArgumentException("Balance cannot be negative");
        }
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.balance = balance;
    }
    
    // Getters
    public double getBalance() {
        return balance;
    }
    
    // Meaningful methods (not setBalance)
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        balance += amount;
    }
    
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient funds");
        }
        balance -= amount;
    }
}
```

**Invariants Enforced:**
- accountNumber is never null or blank
- owner is never null or blank
- balance is never negative

---

## Quick Quiz Questions to Practice

### Conceptual Questions:

1. **Q:** What is encapsulation?  
   **A:** Hiding internal state, exposing behavior, controlling access through methods

2. **Q:** What are the four access modifiers in Java?  
   **A:** public, private, protected, package-private (default)

3. **Q:** Which access modifier is most restrictive?  
   **A:** private

4. **Q:** What's the difference between a field and an invariant?  
   **A:** A field stores state. An invariant is a rule/constraint about that state.

5. **Q:** What does "fail fast" mean?  
   **A:** Detecting invalid input as early as possible and throwing an exception immediately

6. **Q:** What are the three purposes of a constructor?  
   **A:** Initialize state, enforce invariants, runs exactly once

7. **Q:** Why avoid generic setters?  
   **A:** They expose internal state and weaken encapsulation. Use meaningful methods instead.

8. **Q:** What's better: `setCompleted(true)` or `markCompleted()`?  
   **A:** `markCompleted()` - more meaningful and intention-revealing

9. **Q:** What does `this` refer to?  
   **A:** The current object

10. **Q:** Why make fields private by default?  
    **A:** Protects invariants, reduces coupling, enables safe refactoring

### Code Questions:

11. **Q:** Fix this code:
```java
public class Person {
    public String name;
    public int age;
}
```
**A:**
```java
public class Person {
    private String name;
    private int age;
    
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

12. **Q:** What's wrong with this constructor?
```java
public Task(String description) {
    description = description;
}
```
**A:** Missing `this` keyword. Should be: `this.description = description;`

13. **Q:** Add validation to this constructor:
```java
public Task(String description) {
    this.description = description;
}
```
**A:**
```java
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException();
    }
    this.description = description;
}
```

14. **Q:** Which access modifier for this field?
```java
_______ String name;
```
**A:** `private` (fields should be private by default)

15. **Q:** Convert this setter to a meaningful method:
```java
public void setCompleted(boolean completed) {
    this.completed = completed;
}
```
**A:**
```java
public void markCompleted() {
    completed = true;
}

public void markIncomplete() {
    completed = false;
}
```

---

## Key Phrases to Remember

These exact phrases from the slides might appear on the quiz:

1. "A class invariant is a condition that must hold for every valid instance"
2. "Fail fast means detecting invalid input as early as possible"
3. "Getters are not evil, but should be used intentionally"
4. "Setters expose internal state and weaken encapsulation"
5. "private is the default choice"
6. "Encapsulation is impossible without access modifiers"
7. "Access modifiers enforce boundaries"
8. "Visibility reflects responsibility"
9. "Invariants are rules, not fields"
10. "this refers to the current object"

---

## Common Mistakes to Avoid on Quiz

### ❌ Mistake 1: Confusing Fields and Invariants
**Wrong:** "An invariant is a private field"
**Right:** "An invariant is a rule about the state that must always be true"

### ❌ Mistake 2: Wrong Access Modifier Order
**Wrong:** private < protected < public < package-private
**Right:** private < package-private < protected < public (most to least restrictive)

### ❌ Mistake 3: Forgetting `this`
**Wrong:** 
```java
public Person(String name) {
    name = name;  // Ambiguous!
}
```
**Right:**
```java
public Person(String name) {
    this.name = name;  // Clear!
}
```

### ❌ Mistake 4: No Validation
**Wrong:**
```java
public Task(String description) {
    this.description = description;
}
```
**Right:**
```java
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException();
    }
    this.description = description;
}
```

### ❌ Mistake 5: Public Fields
**Wrong:**
```java
public class Task {
    public String description;  // Bad!
}
```
**Right:**
```java
public class Task {
    private String description;  // Good!
    public String getDescription() { return description; }
}
```

---

## Study Strategy for Tonight

### 1. Memorize These (30 minutes):
- [ ] Four access modifiers and their visibility
- [ ] Three principles of encapsulation
- [ ] Three purposes of constructors
- [ ] Four uses of `this`
- [ ] Definition of class invariant
- [ ] Definition of fail fast

### 2. Practice Writing Code (45 minutes):
- [ ] Write a complete Task class from scratch
- [ ] Write a complete BankAccount class from scratch
- [ ] Fix broken code examples
- [ ] Add validation to constructors

### 3. Review Terminology (15 minutes):
- [ ] Encapsulation
- [ ] Invariant
- [ ] Constructor
- [ ] Access modifier
- [ ] Fail fast
- [ ] Anemic object

### 4. Answer Practice Questions (30 minutes):
- [ ] Go through all 15 quick quiz questions above
- [ ] Time yourself
- [ ] Check your answers

---

## Final Checklist - Can You Answer These?

- [ ] What is encapsulation?
- [ ] What are the four access modifiers?
- [ ] What is a class invariant?
- [ ] What does a constructor do?
- [ ] What is the `this` keyword?
- [ ] What does "fail fast" mean?
- [ ] Why avoid setters?
- [ ] Why make fields private?
- [ ] What's package-private?
- [ ] What's protected used for?
- [ ] Name three encapsulation benefits
- [ ] Name three problems with public fields
- [ ] Name three common access modifier mistakes
- [ ] Can you write a complete Task class?
- [ ] Can you add validation to a constructor?

---

## Good Luck on Your Quiz! 🎯

**Key Points to Remember:**
1. **private** is the default for fields
2. **Encapsulation** = hide state, expose behavior, control access
3. **Invariants** = rules that must always be true
4. **Constructors** = initialize, validate, enforce invariants
5. **this** = current object
6. **Fail fast** = validate early, throw exceptions
7. **Meaningful methods** > generic setters
8. **Getters** = use intentionally

You've got this! 🚀