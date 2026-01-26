# Week 6: Encapsulation, Constructors, and Immutability

## What You'll Learn This Week
- **Encapsulation** - Hiding data with private/public
- **Constructors** - Initializing objects properly
- **Getters and Setters** - Controlled access to data
- **Access Modifiers** - public, private, protected
- **Immutability** - Creating unchangeable objects
- **Naming Conventions** - Writing professional code
- **Packages** - Organizing your code
- **Avoiding Anemic Objects** - Data + Behavior together

---

## Procedural vs Object-Oriented Thinking

### The Mindset Shift

**Procedural Thinking:** Focus on steps (what to do)
```java
// Procedural: series of steps
readStudentData();
validateStudentData();
calculateGPA();
saveStudentData();
```

**Object-Oriented Thinking:** Focus on responsibilities (who does what)
```java
// OOP: objects with responsibilities
Student student = new Student("Alice", 20);
student.enrollInCourse(course);
student.calculateGPA();
student.save();
```

**Key Difference:**
- Procedural: "What steps do I need to perform?"
- OOP: "What objects exist and what are they responsible for?"

---

## Day 1-2: Encapsulation

### What is Encapsulation?

**Encapsulation** means:
1. **Hiding** the internal data of an object
2. **Controlling** access through methods
3. **Protecting** data from invalid changes

**Real-World Analogy:**
Think of a TV remote:
- You press buttons (public interface)
- You don't mess with the internal circuits (private data)
- The remote controls access to the TV's functions

### The Problem: Public Variables

**Bad Example (from your professor's slides):**
```java
public class Student {
    public String name;
    public int id;
}
```

**Problems with this:**
```java
Student student = new Student();
student.name = "";  // Empty name? That's invalid!
student.id = -5;    // Negative ID? That makes no sense!
student.id = 999999999;  // Someone can set anything!
```

**Issues:**
- No validation
- Anyone can change anything
- No control over the data
- Hard to maintain
- This creates **anemic objects** (zombie objects - data without behavior)

### The Solution: Private Variables + Methods

**Good Example (the "Better Version" from slides):**
```java
public class Student {
    private String name;  // Hidden from outside
    private int id;       // Protected
    
    // Public method to access the name
    public String getName() {
        return name;
    }
    
    // Public method to set the name (with validation!)
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        } else {
            System.out.println("Invalid name!");
        }
    }
}
```

**Benefits:**
- Data is protected
- Validation happens in one place
- Can change internal implementation later
- Control over what's allowed

### Access Modifiers

Java has four access modifiers:

| Modifier | Access Level |
|----------|-------------|
| `public` | Accessible from anywhere |
| `private` | Only within the same class |
| `protected` | Within package + subclasses (Week 8) |
| (default) | Within the same package only |

**For now, remember:**
- **Instance variables:** Make them `private`
- **Methods:** Make them `public` (if they're part of the interface)

### Complete Encapsulation Example

```java
public class BankAccount {
    // Private data (encapsulated)
    private String accountNumber;
    private String owner;
    private double balance;
    
    // Public methods (interface to interact with the account)
    
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getOwner() {
        return owner;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public void setOwner(String owner) {
        if (owner != null && !owner.isEmpty()) {
            this.owner = owner;
        }
    }
    
    // Business logic methods
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount);
        } else {
            System.out.println("Invalid deposit amount!");
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrew: $" + amount);
        } else {
            System.out.println("Invalid withdrawal!");
        }
    }
}
```

**Key Principle: Behavior Moves With Data**

Notice how `deposit()` and `withdraw()` are in the `BankAccount` class, not in some separate "BankAccountManager" class. The data (balance) and the behavior (deposit/withdraw) belong together!

**Avoid Anemic Objects:**
- ❌ **Anemic Object:** Just data, no behavior (like a zombie 🧟)
- ✅ **Rich Object:** Data + behavior that operates on that data

---

## Day 3-4: Constructors

### What is a Constructor?

A **constructor** is a special method that:
1. Has the **same name as the class**
2. Has **no return type** (not even void)
3. **Initializes** the object when it's created
4. Runs automatically when you use `new`

### The Problem: Uninitialized Objects

**Without constructor:**
```java
public class Student {
    private String name;
    private int id;
    
    // No constructor
}

// Using it:
Student student = new Student();
// name is null, id is 0
// We have to manually set everything:
student.setName("Alice");
student.setId(12345);
// Tedious and error-prone!
```

### The Solution: Constructor

```java
public class Student {
    private String name;
    private int id;
    
    // Constructor!
    public Student(String name, int id) {
        this.name = name;
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public int getId() {
        return id;
    }
}

// Using it:
Student student = new Student("Alice", 12345);
// Done! Object is fully initialized in one line
```

### Constructor Syntax

```java
public ClassName(parameters) {
    // Initialization code
}
```

**Example:**
```java
public class Person {
    private String name;
    private int age;
    
    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### Default Constructor

If you don't write ANY constructor, Java provides a **default constructor**:

```java
public class Person {
    private String name;
    private int age;
}

// Java automatically creates:
// public Person() { }

Person p = new Person();  // Works!
```

**Important:** If you write ANY constructor, the default one goes away!

```java
public class Person {
    private String name;
    
    public Person(String name) {
        this.name = name;
    }
}

Person p = new Person();  // ❌ ERROR! No default constructor
Person p = new Person("Alice");  // ✅ Works!
```

### Constructor Overloading

You can have **multiple constructors** with different parameters:

```java
public class Student {
    private String name;
    private int id;
    private double gpa;
    
    // Constructor 1: All parameters
    public Student(String name, int id, double gpa) {
        this.name = name;
        this.id = id;
        this.gpa = gpa;
    }
    
    // Constructor 2: Just name and id
    public Student(String name, int id) {
        this.name = name;
        this.id = id;
        this.gpa = 0.0;  // Default GPA
    }
    
    // Constructor 3: Just name
    public Student(String name) {
        this.name = name;
        this.id = 0;
        this.gpa = 0.0;
    }
}

// Using them:
Student s1 = new Student("Alice", 12345, 3.8);
Student s2 = new Student("Bob", 67890);
Student s3 = new Student("Charlie");
```

### Constructor Chaining

One constructor can call another using `this()`:

```java
public class Student {
    private String name;
    private int id;
    private double gpa;
    
    // Main constructor
    public Student(String name, int id, double gpa) {
        this.name = name;
        this.id = id;
        this.gpa = gpa;
    }
    
    // This constructor calls the main one
    public Student(String name, int id) {
        this(name, id, 0.0);  // Calls the 3-parameter constructor
    }
    
    public Student(String name) {
        this(name, 0, 0.0);  // Calls the 3-parameter constructor
    }
}
```

### Validation in Constructors

**Always validate in constructors!**

```java
public class BankAccount {
    private String accountNumber;
    private double balance;
    
    public BankAccount(String accountNumber, double initialBalance) {
        if (accountNumber == null || accountNumber.isEmpty()) {
            throw new IllegalArgumentException("Account number cannot be empty");
        }
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Initial balance cannot be negative");
        }
        
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }
}
```

### Complete Constructor Example

```java
public class Car {
    private String brand;
    private String model;
    private int year;
    private double price;
    
    // Full constructor with validation
    public Car(String brand, String model, int year, double price) {
        if (brand == null || brand.isEmpty()) {
            throw new IllegalArgumentException("Brand cannot be empty");
        }
        if (year < 1900 || year > 2025) {
            throw new IllegalArgumentException("Invalid year");
        }
        if (price < 0) {
            throw new IllegalArgumentException("Price cannot be negative");
        }
        
        this.brand = brand;
        this.model = model;
        this.year = year;
        this.price = price;
    }
    
    // Convenient constructor with default price
    public Car(String brand, String model, int year) {
        this(brand, model, year, 0.0);
    }
    
    // Getters
    public String getBrand() {
        return brand;
    }
    
    public String getModel() {
        return model;
    }
    
    public int getYear() {
        return year;
    }
    
    public double getPrice() {
        return price;
    }
    
    // Setter with validation
    public void setPrice(double price) {
        if (price >= 0) {
            this.price = price;
        }
    }
    
    public void displayInfo() {
        System.out.println(year + " " + brand + " " + model + " - $" + price);
    }
}
```

---

## Day 5: Getters and Setters

### What are Getters and Setters?

**Getter:** A method that returns the value of a private variable
**Setter:** A method that sets the value of a private variable

### Why Use Them?

Instead of public variables:
```java
public class Person {
    public String name;  // ❌ Anyone can change this!
}

person.name = "";  // Oops, set to empty!
```

Use private variables with getters/setters:
```java
public class Person {
    private String name;  // ✅ Protected
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        }
    }
}
```

### Naming Conventions (From Your Professor's Slides)

**Getters:**
- Pattern: `get` + `VariableName` (capitalized)
- Example: `getName()`, `getAge()`, `getBalance()`

**Setters:**
- Pattern: `set` + `VariableName` (capitalized)
- Example: `setName()`, `setAge()`, `setBalance()`

**Boolean Getters:**
- Pattern: `is` + `VariableName` (capitalized)
- Example: `isCompleted()`, `isActive()`, `hasPermission()`

### Complete Example

```java
public class Task {
    private String description;
    private boolean completed;
    private int priority;
    
    public Task(String description) {
        this.description = description;
        this.completed = false;
        this.priority = 1;
    }
    
    // Getter for description
    public String getDescription() {
        return description;
    }
    
    // Setter for description (with validation)
    public void setDescription(String description) {
        if (description != null && !description.isEmpty()) {
            this.description = description;
        }
    }
    
    // Boolean getter (uses "is" prefix)
    public boolean isCompleted() {
        return completed;
    }
    
    // Method to mark complete (better than setter!)
    public void markCompleted() {
        this.completed = true;
    }
    
    public int getPriority() {
        return priority;
    }
    
    public void setPriority(int priority) {
        if (priority >= 1 && priority <= 5) {
            this.priority = priority;
        }
    }
}
```

### When NOT to Use Setters

Sometimes, setters don't make sense:

```java
public class BankAccount {
    private String accountNumber;
    private double balance;
    
    // NO setter for accountNumber - it should never change!
    public String getAccountNumber() {
        return accountNumber;
    }
    
    // NO direct setter for balance - use deposit/withdraw instead!
    public double getBalance() {
        return balance;
    }
    
    // Use specific methods instead
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
}
```

---

## Day 6-7: Immutability

### What is Immutability?

An **immutable object** is an object whose state **cannot be changed** after it's created.

### Why Immutability?

**Benefits:**
1. **Thread-safe** - Multiple threads can use it without problems
2. **Predictable** - Object won't change unexpectedly
3. **Cacheable** - Safe to reuse
4. **Simpler** - No need to worry about changes

**Real-World Examples:**
- `String` in Java is immutable
- Numbers are immutable
- Dates (in modern Java) are immutable

### Creating Immutable Classes

**Rules for Immutable Classes:**
1. Make all fields `private` and `final`
2. No setters!
3. Initialize all fields in constructor
4. Don't expose mutable objects

### Example 1: Immutable Point

```java
public class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() {
        return x;
    }
    
    public int getY() {
        return y;
    }
    
    // No setters! Object cannot change
    
    // If you need a "modified" version, return a NEW object
    public Point move(int dx, int dy) {
        return new Point(x + dx, y + dy);
    }
}
```

**Using it:**
```java
Point p1 = new Point(5, 10);
System.out.println(p1.getX());  // 5

// Can't modify p1!
// p1.setX(20);  // ❌ No such method!

// To get a different point, create a new one
Point p2 = p1.move(3, 5);
System.out.println(p2.getX());  // 8
System.out.println(p1.getX());  // Still 5! Original unchanged
```

### The `final` Keyword

`final` means "cannot be changed":

**For variables:**
```java
final int MAX_SIZE = 100;
MAX_SIZE = 200;  // ❌ ERROR! Cannot change

final String name = "Alice";
name = "Bob";  // ❌ ERROR!
```

**For instance variables:**
```java
public class Person {
    private final String name;  // Must be set in constructor
    
    public Person(String name) {
        this.name = name;  // OK - set once
    }
    
    public void changeName(String newName) {
        this.name = newName;  // ❌ ERROR! Cannot change final field
    }
}
```

### Example 2: Immutable Date

```java
public class Date {
    private final int day;
    private final int month;
    private final int year;
    
    public Date(int day, int month, int year) {
        // Validation
        if (day < 1 || day > 31) {
            throw new IllegalArgumentException("Invalid day");
        }
        if (month < 1 || month > 12) {
            throw new IllegalArgumentException("Invalid month");
        }
        if (year < 1900) {
            throw new IllegalArgumentException("Invalid year");
        }
        
        this.day = day;
        this.month = month;
        this.year = year;
    }
    
    public int getDay() {
        return day;
    }
    
    public int getMonth() {
        return month;
    }
    
    public int getYear() {
        return year;
    }
    
    // Return a NEW date, don't modify this one
    public Date addDays(int days) {
        // Simplified - just adds to day
        return new Date(day + days, month, year);
    }
    
    @Override
    public String toString() {
        return month + "/" + day + "/" + year;
    }
}
```

### String Immutability

`String` is immutable in Java:

```java
String s1 = "Hello";
String s2 = s1.toUpperCase();  // Creates NEW string

System.out.println(s1);  // "Hello" - unchanged!
System.out.println(s2);  // "HELLO" - new string
```

**This is why we use:**
```java
s1 = s1 + " World";  // Creates new string, assigns to s1
// Old "Hello" string is unchanged (and garbage collected)
```

### Mutable vs Immutable

**Mutable Example:**
```java
public class Counter {
    private int count;
    
    public Counter(int count) {
        this.count = count;
    }
    
    public void increment() {
        count++;  // ✅ Changes the object
    }
    
    public int getCount() {
        return count;
    }
}

Counter c = new Counter(5);
c.increment();
c.increment();
System.out.println(c.getCount());  // 7 - object changed!
```

**Immutable Example:**
```java
public class Counter {
    private final int count;
    
    public Counter(int count) {
        this.count = count;
    }
    
    public Counter increment() {
        return new Counter(count + 1);  // Returns NEW object
    }
    
    public int getCount() {
        return count;
    }
}

Counter c1 = new Counter(5);
Counter c2 = c1.increment();
Counter c3 = c2.increment();
System.out.println(c1.getCount());  // 5 - unchanged!
System.out.println(c3.getCount());  // 7 - new object
```

---

## Java Naming Conventions

### Class Names: PascalCase

Every word capitalized, no spaces:
```java
public class BankAccount { }
public class StudentRecord { }
public class TaskManager { }
public class ShoppingCart { }
```

### Method and Variable Names: camelCase

First word lowercase, rest capitalized:
```java
// Methods
public void calculateTotal() { }
public void addTask() { }
public void markComplete() { }

// Variables
int accountBalance;
String firstName;
double totalPrice;
```

### Constants: UPPER_SNAKE_CASE

All uppercase, words separated by underscores:
```java
public static final int MAX_RETRIES = 3;
public static final double DEFAULT_TIMEOUT_MS = 1000.0;
public static final String API_KEY = "abc123";
```

### Packages: lowercase

All lowercase, often reverse domain:
```java
package com.example.app;
package edu.university.course;
package org.company.project;
```

### Naming Guidelines That Improve Design

**Classes - Use Nouns:**
✅ `Task`, `Invoice`, `Repository`, `Student`
❌ `DoSomething`, `Manager`, `Helper`

**Methods - Use Verbs:**
✅ `addTask()`, `removeTask()`, `markComplete()`
❌ `task()`, `data()`, `thing()`

**Boolean Variables - Use "is" or "has":**
✅ `isCompleted`, `hasPermission`, `isActive`
❌ `completed`, `permission`, `active`

**Be Specific:**
✅ `TaskList` (clear what kind of list)
❌ `List` (too generic)

**Avoid Abbreviations:**
✅ `calculateTotalAmount()`
❌ `calcTotAmt()`

**Exception: Universally Known Abbreviations:**
✅ `URL`, `HTTP`, `ID`, `HTML`

### Common Naming Pitfalls

❌ **Vague names:**
- `Manager`, `Helper`, `Util`, `Data`
- These tell you nothing about what the class does!

❌ **Overloaded meanings:**
- `handle()`, `process()`, `doIt()`
- What do these actually do?

❌ **Single letter names (outside loops):**
- `x`, `temp`, `a`, `b`
- Use meaningful names!

❌ **Leaking implementation details:**
- `ArrayTaskStorage` when you mean `TaskRepository`
- Don't expose how it's implemented in the name

### Mini Exercise (From Slides)

Rename these to improve clarity:

❌ `doIt()` → ✅ `processPayment()`, `calculateTotal()`, `sendEmail()`
❌ `Data1` → ✅ `StudentData`, `CustomerRecord`, `OrderInfo`
❌ `ThingManager` → ✅ `TaskManager`, `UserController`, `OrderProcessor`
❌ `tList` → ✅ `taskList`, `todoItems`, `pendingTasks`

---

## Packages

### What are Packages?

**Packages** are folders that organize your classes, like folders organize files on your computer.

### Why Use Packages?

1. **Organization** - Group related classes
2. **Avoid name conflicts** - Two classes can have same name in different packages
3. **Access control** - Package-level visibility

### Package Naming Convention

Use reverse domain name:
```
com.company.project
edu.university.course
org.opensource.library
```

### Package Structure Example

```
src/
  └── edu/
      └── university/
          └── taskmanager/
              ├── model/
              │   ├── Task.java
              │   └── TaskList.java
              ├── controller/
              │   └── TaskController.java
              └── Main.java
```

### Declaring a Package

At the **very top** of your Java file:

```java
package edu.university.taskmanager.model;

public class Task {
    private String description;
    private boolean completed;
    
    // ...
}
```

### Using Classes from Other Packages

**Option 1: Import**
```java
package edu.university.taskmanager;

import edu.university.taskmanager.model.Task;
import edu.university.taskmanager.model.TaskList;

public class Main {
    public static void main(String[] args) {
        Task task = new Task("Study OOP");
        TaskList list = new TaskList();
    }
}
```

**Option 2: Fully Qualified Name**
```java
package edu.university.taskmanager;

public class Main {
    public static void main(String[] args) {
        edu.university.taskmanager.model.Task task = 
            new edu.university.taskmanager.model.Task("Study OOP");
    }
}
```

---

## Records (Modern Java Feature)

### What are Records?

A **record** is a special class (Java 16+) for storing **immutable data** with minimal code.

From your professor's slides:
```java
public record Student(String name, int id) {}
```

This ONE line automatically creates:
- Private final fields
- Constructor
- Getters (name(), id())
- equals(), hashCode(), toString()

### Equivalent Regular Class

**Without record (lots of code):**
```java
public class Student {
    private final String name;
    private final int id;
    
    public Student(String name, int id) {
        this.name = name;
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public int getId() {
        return id;
    }
    
    @Override
    public boolean equals(Object obj) {
        // ... lots of code
    }
    
    @Override
    public int hashCode() {
        // ... more code
    }
    
    @Override
    public String toString() {
        return "Student[name=" + name + ", id=" + id + "]";
    }
}
```

**With record (one line!):**
```java
public record Student(String name, int id) {}
```

### Using Records

```java
public record Point(int x, int y) {}

// Create a record
Point p1 = new Point(5, 10);

// Access fields (note: no "get" prefix!)
System.out.println(p1.x());  // 5
System.out.println(p1.y());  // 10

// toString() is automatic
System.out.println(p1);  // Point[x=5, y=10]

// Records are immutable
// p1.x = 20;  // ❌ ERROR! No setters!
```

### When to Use Records

✅ **Use records for:**
- Simple data carriers
- Immutable data
- DTOs (Data Transfer Objects)
- Configuration objects

❌ **Don't use records for:**
- Objects that need to change (mutable)
- Complex business logic
- Objects with many behaviors

**Note:** We'll revisit records later in the course. For now, focus on regular classes!

---

## Week 6 Practice Exercises

### Exercise 1: Immutable Book

Create an **immutable** `Book` class:
- Fields: `title` (String), `author` (String), `isbn` (String), `pages` (int)
- All fields should be `private final`
- Constructor that initializes all fields with validation
- Only getters, NO setters
- Method `isLongBook()` that returns true if pages > 300

**Test it:**
```java
Book book = new Book("Java Programming", "John Doe", "123-456", 450);
System.out.println(book.getTitle());
System.out.println(book.isLongBook());  // true
```

### Exercise 2: BankAccount with Encapsulation

Create a `BankAccount` class with proper encapsulation:
- Private fields: `accountNumber`, `owner`, `balance`
- Constructor that initializes all fields
- Getters for all fields
- NO direct setter for balance
- Methods: `deposit(double amount)`, `withdraw(double amount)`
- Validate that amounts are positive and withdrawals don't exceed balance

### Exercise 3: Person with Multiple Constructors

Create a `Person` class:
- Private fields: `firstName`, `lastName`, `age`, `email`
- Constructor 1: All four parameters
- Constructor 2: Just firstName, lastName, age (email = "")
- Constructor 3: Just firstName and lastName (age = 0, email = "")
- Use constructor chaining
- Getters and setters with validation

### Exercise 4: Product Catalog

Create a `Product` class:
- Private fields: `id`, `name`, `price`, `quantity`
- Constructor with all fields
- Getters for all fields
- Setter for price (validate > 0)
- Methods:
  - `restock(int amount)` - adds to quantity
  - `sell(int amount)` - reduces quantity if available
  - `getTotalValue()` - returns price * quantity
  - `isInStock()` - returns true if quantity > 0

### Exercise 5: Rectangle with Validation

Create a `Rectangle` class with **strong encapsulation**:
- Private fields: `width`, `height`
- Constructor that validates width and height are positive
- Getters only (immutable after creation)
- Methods:
  - `getArea()`
  - `getPerimeter()`
  - `isSquare()` - returns true if width == height
  - `resize(double widthFactor, double heightFactor)` - returns NEW Rectangle

### Exercise 6: Student Record (Using Record)

If your Java version supports it (Java 16+), create a `StudentRecord`:
```java
public record StudentRecord(String name, int id, double gpa) {}
```

Test creating students and accessing their data.

---

## Common Beginner Mistakes

### 1. Making Everything Public

```java
// ❌ BAD
public class Person {
    public String name;
    public int age;
}

// ✅ GOOD
public class Person {
    private String name;
    private int age;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

### 2. No Validation

```java
// ❌ BAD
public void setAge(int age) {
    this.age = age;  // What if age is -5 or 200?
}

// ✅ GOOD
public void setAge(int age) {
    if (age >= 0 && age <= 120) {
        this.age = age;
    } else {
        throw new IllegalArgumentException("Invalid age");
    }
}
```

### 3. Forgetting `this` in Constructor

```java
// ❌ CONFUSING
public Person(String name) {
    name = name;  // Which name?? Both are the parameter!
}

// ✅ CLEAR
public Person(String name) {
    this.name = name;  // this.name is the field
}
```

### 4. Breaking Immutability

```java
// Trying to make immutable class:
public class Point {
    private final int x;
    private final int y;
    
    // ❌ BAD - This breaks immutability!
    public void setX(int x) {
        this.x = x;  // ERROR! Can't modify final field
    }
}
```

### 5. Returning Mutable Objects

```java
public class Person {
    private Date birthDate;  // Date is mutable!
    
    // ❌ BAD - exposes internal mutable object
    public Date getBirthDate() {
        return birthDate;  // Caller can modify it!
    }
    
    // ✅ GOOD - return a copy
    public Date getBirthDate() {
        return new Date(birthDate.getTime());
    }
}
```

---

## Design Principles Recap

### Encapsulation
- Hide data with `private`
- Provide controlled access with methods
- Validate all inputs

### Constructor Design
- Initialize all fields
- Validate parameters
- Use constructor overloading for flexibility
- Use constructor chaining to avoid duplication

### Immutability
- Use `final` for fields that shouldn't change
- No setters for immutable classes
- Return new objects instead of modifying

### Naming
- Follow Java conventions
- Be specific and clear
- Use nouns for classes, verbs for methods

### Avoid Anemic Objects
- Put behavior with data
- Don't create classes that are just data holders
- Make objects responsible for their own operations

---

## Week 6 Checklist

By the end of this week, you should be able to:
- [ ] Understand encapsulation and why it matters
- [ ] Use `private` and `public` correctly
- [ ] Write constructors with validation
- [ ] Overload constructors
- [ ] Chain constructors using `this()`
- [ ] Write proper getters and setters
- [ ] Create immutable classes using `final`
- [ ] Follow Java naming conventions
- [ ] Organize code with packages
- [ ] Understand records (modern Java)
- [ ] Avoid creating anemic objects
- [ ] Complete all 6 practice exercises

---

## Next Week Preview

In Week 7, we'll learn:
- **Object Relationships** - How objects work together
- **Association, Aggregation, Composition**
- **"Has-a" vs "Is-a" relationships**
- **Object Collaboration** - Objects calling each other's methods
- **UML Class Diagrams** - Visualizing relationships

---

## Key Takeaways

**Encapsulation:**
- **Hide** data with private
- **Control** access with methods
- **Validate** everything

**Constructors:**
- **Initialize** objects properly
- **Validate** from the start
- **Chain** constructors to avoid duplication

**Immutability:**
- Use **final** for unchanging data
- **No setters** for immutable objects
- Return **new objects** instead of modifying

**Naming:**
- **PascalCase** for classes
- **camelCase** for methods/variables
- **UPPER_SNAKE_CASE** for constants
- Be **specific** and **clear**

**Design:**
- **Behavior belongs with data**
- Avoid **anemic objects** (zombie classes)
- Think about **responsibilities**

---

You've now learned the fundamental building blocks of professional Java OOP! These concepts (encapsulation, constructors, immutability) are used in EVERY Java application. Master them now and everything else will be easier! 🚀

Ready for Week 7? Let me know when you've completed the Week 6 exercises!