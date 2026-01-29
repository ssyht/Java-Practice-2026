# Week 6 Quiz - Classes, Encapsulation, and Immutability
## 20 Questions

**Instructions:** Answer all questions. Check your answers at the end.

---

## Part 1: Multiple Choice (10 Questions)

### Question 1
What are the three core aspects of a class?

A) Variables, methods, and constructors  
B) ```Represents a domain concept, bundles data and behavior, protects internal state  ``
C) Public fields, private methods, and protected constructors  
D) Inheritance, polymorphism, and encapsulation  

---

### Question 2
Which access modifier is the MOST restrictive?

A) public  
B) protected  
C) ``package-private (default)  ``
D) private  

---

### Question 3
What is a class invariant?

A) A private field that never changes  
B) A final variable  
C) A condition that must hold for every valid instance, before and after every public method call  
D) A static method  

---

### Question 4
What does "fail fast" mean?

A) Completing the program as quickly as possible  
B)`` Detecting invalid input as early as possible and throwing an exception immediately  ``
C) Using the fastest algorithm  
D) Avoiding loops  

---

### Question 5
Which access modifier allows access in subclasses but NOT everywhere?

A) public  
B) private  
C) ``protected  ``
D) package-private  

---

### Question 6
What are the three principles of encapsulation?

A) Abstraction, inheritance, polymorphism  
B)`` Hide internal state, expose behavior, control access through methods  ``
C) Public methods, private fields, protected constructors  
D) Variables, methods, and objects  

---

### Question 7
Why should fields be private by default?

A) It makes code run faster  
B) It's required by Java  
C)`` Protects invariants, reduces coupling, enables safe refactoring  ``
D) It uses less memory  

---

### Question 8
What does the `this` keyword refer to?

A) The parent class  
B) ``The current object  ``
C) The next object  
D) A static variable  

---

### Question 9
Which is better design and why?

A) `setCompleted(true)` - because setters are standard  
``B) `markCompleted()` - because it's more meaningful and intention-revealing  ``
C) `completed = true` - because direct access is faster  
D) `updateStatus(true)` - because it's generic  

---

### Question 10
What's wrong with this code?
```java
public class Task {
    public String description;
    public boolean completed;
}
```

A) Missing constructor  
``B) Public fields: no validation, no control, breaks invariants, hard to refactor `` 
C) Should use int instead of boolean  
D) Nothing is wrong  

---

## Part 2: True/False (5 Questions)

### Question 11
**True or False:** An invariant is the same as a private field.

False

---

### Question 12
**True or False:** Getters are evil and should never be used.

False

---

### Question 13
**True or False:** Constructors should validate parameters to enforce invariants from the start.

False

---

### Question 14
**True or False:** Package-private (default) means accessible only within the same class.

False

---

### Question 15
**True or False:** Setters expose internal state and weaken encapsulation, so meaningful methods are preferred.

True

---

## Part 3: Code Analysis (3 Questions)

### Question 16
What's wrong with this constructor? How would you fix it?

```java
public Task(String description) {
    description = description;
}
```



---

### Question 17
Add proper validation to this constructor:

```java
public BankAccount(String owner, double balance) {
    this.owner = owner;
    this.balance = balance;
}
```

**Invariants to enforce:**
- owner cannot be null or blank
- balance cannot be negative

---

### Question 18
Convert this generic setter to meaningful methods:

```java
public void setCompleted(boolean completed) {
    this.completed = completed;
}
```

---

## Part 4: Short Answer (2 Questions)

### Question 19
Explain the three questions you should ask when identifying class responsibilities.

---

### Question 20
Fill in the access modifier table:

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|------------|--------------|----------|------------|
| private | ? | ? | ? | ? |
| (default) | ? | ? | ? | ? |
| protected | ? | ? | ? | ? |
| public | ? | ? | ? | ? |

---
---
---

# ANSWER KEY

## Part 1: Multiple Choice

### Question 1: **B**
Represents a domain concept, bundles data and behavior, protects internal state

### Question 2: **D**
private

### Question 3: **C**
A condition that must hold for every valid instance, before and after every public method call

### Question 4: **B**
Detecting invalid input as early as possible and throwing an exception immediately

### Question 5: **C**
protected

### Question 6: **B**
Hide internal state, expose behavior, control access through methods

### Question 7: **C**
Protects invariants, reduces coupling, enables safe refactoring

### Question 8: **B**
The current object

### Question 9: **B**
`markCompleted()` - because it's more meaningful and intention-revealing

### Question 10: **B**
Public fields: no validation, no control, breaks invariants, hard to refactor

---

## Part 2: True/False

### Question 11: **FALSE**
An invariant is NOT a field. Fields store state. Invariants are RULES/constraints about that state.

### Question 12: **FALSE**
Getters are NOT evil, but should be used intentionally (from professor's slides).

### Question 13: **TRUE**
Constructors should validate parameters to enforce invariants and fail fast.

### Question 14: **FALSE**
Package-private means accessible within the same PACKAGE, not just the same class.

### Question 15: **TRUE**
This is a direct quote from the professor's slides.

---

## Part 3: Code Analysis

### Question 16 Answer:
**Problem:** Missing `this` keyword - ambiguous which `description` is which.

**Fixed:**
```java
public Task(String description) {
    this.description = description;
}
```

Better with validation:
```java
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException("Description cannot be null or blank");
    }
    this.description = description;
}
```

---

### Question 17 Answer:
```java
public BankAccount(String owner, double balance) {
    if (owner == null || owner.isBlank()) {
        throw new IllegalArgumentException("Owner cannot be null or blank");
    }
    if (balance < 0) {
        throw new IllegalArgumentException("Balance cannot be negative");
    }
    this.owner = owner;
    this.balance = balance;
}
```

---

### Question 18 Answer:
```java
// Instead of generic setter, use meaningful methods:

public void markCompleted() {
    completed = true;
}

public void markIncomplete() {
    completed = false;
}

// Or if you need more control:
public void markCompleted() {
    if (!completed) {
        completed = true;
        // Could add logging, notifications, etc.
    }
}
```

---

## Part 4: Short Answer

### Question 19 Answer:
The three questions for class responsibilities:
1. **What does this class know?** (Data/State/Fields)
2. **What does this class do?** (Behavior/Methods/Actions)
3. **What does this class NOT do?** (Boundaries/Limits/What's outside its responsibility)

---

### Question 20 Answer:

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|------------|--------------|----------|------------|
| private | ✅ | ❌ | ❌ | ❌ |
| (default) | ✅ | ✅ | ❌ | ❌ |
| protected | ✅ | ✅ | ✅ | ❌ |
| public | ✅ | ✅ | ✅ | ✅ |

---

## Scoring Guide

**18-20 correct:** Excellent! You're ready for the quiz! 🎯  
**15-17 correct:** Good! Review the ones you missed.  
**12-14 correct:** Study more tonight, focus on weak areas.  
**Below 12:** Go through the Quiz Preparation Guide again carefully.

---

## Key Concepts to Review if You Missed Them

**Missed Q1, Q19?** → Review: What is a class, class responsibilities  
**Missed Q2, Q5, Q20?** → Review: Access modifiers table  
**Missed Q3, Q11?** → Review: Class invariants (rules vs fields)  
**Missed Q4, Q13?** → Review: Fail fast principle, constructor validation  
**Missed Q6?** → Review: Three principles of encapsulation  
**Missed Q7?** → Review: Why private is default choice  
**Missed Q8, Q16?** → Review: `this` keyword  
**Missed Q9, Q15, Q18?** → Review: Meaningful methods vs generic setters  
**Missed Q10?** → Review: Problems with public fields  
**Missed Q12?** → Review: When to use getters  
**Missed Q14?** → Review: Package-private visibility  
**Missed Q17?** → Review: Constructor validation  

---

## Quick Review Before Quiz

**Must Know:**
1. Four access modifiers: private, (default), protected, public
2. Encapsulation = hide state, expose behavior, control access
3. Invariants = rules that must always be true (not fields!)
4. Constructors = initialize, enforce invariants, run once
5. `this` = current object
6. Fail fast = validate early, throw exceptions
7. Meaningful methods > generic setters
8. private is default for fields

**Good luck on your actual quiz tomorrow! 🚀**