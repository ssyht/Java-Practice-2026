# Final Practice Test - 25 Questions
## Lectures 1 & 2: Java OOP Foundations, Classes, Encapsulation, and Immutability

**Time Limit:** 30 minutes  
**Instructions:** Answer all questions. Check your answers at the end.

---

## Part 1: Multiple Choice (15 Questions)

### Question 1
What are the three core aspects of a class?

A) Inheritance, polymorphism, and encapsulation  
B) Represents a domain concept, bundles data and behavior, protects internal state  
C) Variables, methods, and constructors  
D) Public, private, and protected  

---

### Question 2
Which naming convention is CORRECT?

A) Class: `bankAccount`, Method: `CalculateTotal`, Constant: `maxRetries`  
B) Class: `BankAccount`, Method: `calculateTotal`, Constant: `MAX_RETRIES`  
C) Class: `bank_account`, Method: `calculate_total`, Constant: `MaxRetries`  
D) Class: `BANKACCOUNT`, Method: `CALCULATETOTAL`, Constant: `max_retries`  

---

### Question 3
What are the three questions to ask when identifying class responsibilities?

A) How, when, and why  
B) Public, private, or protected  
C) What does it know? What does it do? What does it NOT do?  
D) Create, read, update, or delete  

---

### Question 4
Which access modifier is MOST restrictive?

A) public  
B) protected  
C) package-private (default)  
D) private  

---

### Question 5
What is a class invariant?

A) A final variable that never changes  
B) A condition that must hold for every valid instance, before and after every public method call  
C) A private field  
D) A static method  

---

### Question 6
What are the three principles of encapsulation?

A) Hide internal state, expose behavior, control access through methods  
B) Create objects, call methods, return values  
C) Public fields, private methods, protected constructors  
D) Inheritance, interfaces, abstraction  

---

### Question 7
According to the slides, which statement about getters is TRUE?

A) Getters are evil and should never be used  
B) Getters are not evil, but should be used intentionally  
C) Every field should automatically have a getter  
D) Getters should always be private  

---

### Question 8
Why should fields be private by default?

A) It makes code run faster  
B) It's required by Java syntax  
C) Protects invariants, reduces coupling, enables safe refactoring  
D) It saves memory  

---

### Question 9
What does the `this` keyword refer to?

A) The parent class  
B) The current object  
C) The previous object  
D) A static variable  

---

### Question 10
What does "fail fast" mean?

A) Complete the program quickly  
B) Detecting invalid input as early as possible and throwing an exception immediately  
C) Use the fastest sorting algorithm  
D) Avoid using loops  

---

### Question 11
Which is better design?

A) `setBalance(double)` - because setters are standard practice  
B) `deposit(double)` and `withdraw(double)` - because they're meaningful and can enforce rules  
C) `balance = amount` - because direct access is fastest  
D) `updateMoney(double)` - because it's generic  

---

### Question 12
What's wrong with this code?
```java
public class Task {
    public String description;
    public boolean completed;
}
```

A) Missing main method  
B) Public fields: no validation, no control, breaks invariants, hard to refactor  
C) Should use int instead of boolean  
D) Nothing is wrong  

---

### Question 13
Which access modifier allows access in subclasses but NOT everywhere?

A) public  
B) private  
C) protected  
D) package-private  

---

### Question 14
What are the three purposes of a constructor?

A) Create variables, initialize arrays, call methods  
B) Initialize object state, enforce invariants, runs exactly once  
C) Public interface, private implementation, protected inheritance  
D) Read data, write data, delete data  

---

### Question 15
Which is a common naming pitfall to AVOID?

A) Using PascalCase for classes  
B) Using camelCase for methods  
C) Using vague names like Manager, Helper, Util  
D) Using UPPER_SNAKE_CASE for constants  

---

## Part 2: Access Modifier Table (1 Question)

### Question 16
Fill in the access modifier visibility table with ✅ or ❌:

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|------------|--------------|----------|------------|
| private | ? | ? | ? | ? |
| (default) | ? | ? | ? | ? |
| protected | ? | ? | ? | ? |
| public | ? | ? | ? | ? |

---

## Part 3: True/False (5 Questions)

### Question 17
**True or False:** An invariant is the same thing as a private field.

---

### Question 18
**True or False:** Constructors should validate parameters to enforce invariants from the start (fail fast principle).

---

### Question 19
**True or False:** Setters expose internal state and weaken encapsulation, so meaningful methods like `markCompleted()` are preferred over generic setters like `setCompleted(boolean)`.

---

### Question 20
**True or False:** Procedural programming focuses on responsibilities (who does what), while OOP focuses on steps (what to do).

---

### Question 21
**True or False:** Package-private (default) visibility means accessible only within the same class.

---

## Part 4: Code Analysis (4 Questions)

### Question 22
What's wrong with this constructor? How would you fix it?

```java
public Task(String description) {
    description = description;
}
```

**Answer:**

---

### Question 23
Add proper validation to enforce the invariants in this constructor:

```java
public BankAccount(String owner, double balance) {
    this.owner = owner;
    this.balance = balance;
}
```

**Invariants:**
- owner cannot be null or blank
- balance cannot be negative

**Your answer:**

---

### Question 24
Convert this generic setter into meaningful method(s):

```java
public void setCompleted(boolean completed) {
    this.completed = completed;
}
```

**Your answer:**

---

### Question 25
Identify the class invariants for this Person class:

```java
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        if (name == null || name.isEmpty()) {
            throw new IllegalArgumentException();
        }
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException();
        }
        this.name = name;
        this.age = age;
    }
}
```

**What are the invariants (the rules)?**

---
---
---

# ANSWER KEY

## Part 1: Multiple Choice

### Question 1: **B**
Represents a domain concept, bundles data and behavior, protects internal state

**Explanation:** From Lecture 2, slide "What Is a Class Really?"

---

### Question 2: **B**
Class: `BankAccount`, Method: `calculateTotal`, Constant: `MAX_RETRIES`

**Explanation:** 
- Classes: PascalCase
- Methods/variables: camelCase
- Constants: UPPER_SNAKE_CASE

---

### Question 3: **C**
What does it know? What does it do? What does it NOT do?

**Explanation:** The three class responsibility questions from Lecture 2.

---

### Question 4: **D**
private

**Explanation:** private is the most restrictive - only accessible within the same class.

---

### Question 5: **B**
A condition that must hold for every valid instance, before and after every public method call

**Explanation:** Exact definition from the slides. An invariant is a RULE, not a field.

---

### Question 6: **A**
Hide internal state, expose behavior, control access through methods

**Explanation:** The three principles of encapsulation from Lecture 2.

---

### Question 7: **B**
Getters are not evil, but should be used intentionally

**Explanation:** Direct quote from professor's slides - don't automatically add getters for every field.

---

### Question 8: **C**
Protects invariants, reduces coupling, enables safe refactoring

**Explanation:** The three reasons why private is the default choice from the slides.

---

### Question 9: **B**
The current object

**Explanation:** `this` refers to the current object being constructed or operated on.

---

### Question 10: **B**
Detecting invalid input as early as possible and throwing an exception immediately

**Explanation:** The exact definition of "fail fast" from the slides.

---

### Question 11: **B**
`deposit(double)` and `withdraw(double)` - because they're meaningful and can enforce rules

**Explanation:** This is the "Quick Check" example from the slides - meaningful methods are better than generic setters.

---

### Question 12: **B**
Public fields: no validation, no control, breaks invariants, hard to refactor

**Explanation:** The four problems with public fields from Lecture 2.

---

### Question 13: **C**
protected

**Explanation:** protected allows access in same class, same package, AND subclasses, but not everywhere.

---

### Question 14: **B**
Initialize object state, enforce invariants, runs exactly once

**Explanation:** The three purposes of a constructor from the slides.

---

### Question 15: **C**
Using vague names like Manager, Helper, Util

**Explanation:** From Lecture 1 "Common Naming Pitfalls" - these are vague and tell you nothing about what the class does.

---

## Part 2: Access Modifier Table

### Question 16 Answer:

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|------------|--------------|----------|------------|
| private | ✅ | ❌ | ❌ | ❌ |
| (default) | ✅ | ✅ | ❌ | ❌ |
| protected | ✅ | ✅ | ✅ | ❌ |
| public | ✅ | ✅ | ✅ | ✅ |

**Memorize this table!**

---

## Part 3: True/False

### Question 17: **FALSE**
An invariant is NOT a field. Fields store state. Invariants are RULES/constraints about that state.

**From slides:** "An invariant is not a field. Fields store state. Invariants constrain that state."

---

### Question 18: **TRUE**
Constructors should validate parameters to enforce invariants and fail fast.

**From slides:** Validation in constructors follows the "fail fast" principle.

---

### Question 19: **TRUE**
This is directly from the professor's slides: "Setters expose internal state and weaken encapsulation. Prefer meaningful methods."

---

### Question 20: **FALSE**
It's the opposite! 
- **Procedural:** focuses on steps (what to do)
- **OOP:** focuses on responsibilities (who does what)

---

### Question 21: **FALSE**
Package-private means accessible within the same PACKAGE, not just the same class.

---

## Part 4: Code Analysis

### Question 22 Answer:

**Problem:** Missing `this` keyword - ambiguous which `description` is which. Sets parameter to itself, not the field!

**Fixed:**
```java
public Task(String description) {
    this.description = description;
}
```

**Even better with validation:**
```java
public Task(String description) {
    if (description == null || description.isBlank()) {
        throw new IllegalArgumentException("Description cannot be null or blank");
    }
    this.description = description;
}
```

---

### Question 23 Answer:

```java
public BankAccount(String owner, double balance) {
    // Validate owner (enforce invariant)
    if (owner == null || owner.isBlank()) {
        throw new IllegalArgumentException("Owner cannot be null or blank");
    }
    
    // Validate balance (enforce invariant)
    if (balance < 0) {
        throw new IllegalArgumentException("Balance cannot be negative");
    }
    
    this.owner = owner;
    this.balance = balance;
}
```

**Key points:**
- Validate BEFORE initializing
- Check both invariants
- Throw exceptions for invalid input
- Fail fast!

---

### Question 24 Answer:

```java
// Instead of generic setter, use meaningful methods:

public void markCompleted() {
    completed = true;
}

public void markIncomplete() {
    completed = false;
}
```

**Why this is better:**
- Clear intention: `markCompleted()` vs `setCompleted(true)`
- More readable
- Can add validation/logging later
- Follows the principle from slides: "prefer meaningful methods"

---

### Question 25 Answer:

**The invariants (rules) are:**
1. **name is never null**
2. **name is never empty**
3. **age is between 0 and 150** (inclusive)

**These are RULES, not fields!**
- The fields are: `name` and `age` (store data)
- The invariants are: the constraints on what values those fields can have

---

## Scoring Guide

**23-25 correct:** Excellent! You're completely ready! 🎯  
**20-22 correct:** Very good! Review the ones you missed.  
**17-19 correct:** Good! Study weak areas tonight.  
**14-16 correct:** Study more - focus on key concepts.  
**Below 14:** Go through the Quick Review again carefully.

---

## Topic Review Based on Mistakes

**Missed Q1?** → Review: Three aspects of a class  
**Missed Q2, Q15?** → Review: Naming conventions  
**Missed Q3?** → Review: Class responsibilities  
**Missed Q4, Q13, Q16?** → Review: Access modifiers table  
**Missed Q5, Q17, Q25?** → Review: Class invariants (rules vs fields)  
**Missed Q6?** → Review: Three encapsulation principles  
**Missed Q7, Q19, Q24?** → Review: Getters/setters, meaningful methods  
**Missed Q8?** → Review: Why private is default  
**Missed Q9, Q22?** → Review: `this` keyword  
**Missed Q10, Q18, Q23?** → Review: Fail fast, constructor validation  
**Missed Q11?** → Review: Meaningful methods vs generic setters  
**Missed Q12?** → Review: Problems with public fields  
**Missed Q14?** → Review: Three purposes of constructors  
**Missed Q20?** → Review: Procedural vs OOP thinking  
**Missed Q21?** → Review: Package-private visibility  

---

## Final Reminders Before Quiz

### Must Know By Heart:

1. **Three aspects of a class**
2. **Three class responsibility questions**
3. **Three encapsulation principles**
4. **Four access modifiers + visibility table**
5. **Class invariant definition**
6. **Three constructor purposes**
7. **Four uses of `this`**
8. **Fail fast definition**
9. **Why private is default (3 reasons)**
10. **Problems with public fields (4 problems)**

### Key Phrases:
- "A class invariant is a condition that must hold..."
- "Fail fast means detecting invalid input as early as possible..."
- "Getters are not evil, but should be used intentionally"
- "private is the default choice"
- "Invariants are rules, not fields"

### Code Skills:
- Write a Task class with validation
- Add constructor validation
- Fix missing `this`
- Convert setters to meaningful methods
- Identify invariants from code

---

## You're Ready! 🚀

**Get a good night's sleep and ace that quiz tomorrow!**

Remember:
- Read questions carefully
- If stuck, move on and come back
- Check your work if time permits
- Trust your preparation!

**Good luck! You've got this! 🎯**