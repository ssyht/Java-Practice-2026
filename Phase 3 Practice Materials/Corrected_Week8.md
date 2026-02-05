# Week 8: Object Relationships and Collaboration

## Overview

In object-oriented programming, objects rarely exist in isolation. Understanding how classes form relationships and how objects collaborate is fundamental to good software design.

### Key Concepts:
- Objects rarely live alone
- Classes form relationships
- Design is about collaboration
- Focus on how objects connect and communicate

## Learning Goals

By the end of this week, you should be able to:
- Identify relationships between classes
- Choose between association, aggregation, and composition
- Design object collaboration responsibly
- Apply the Law of Demeter
- Improve designs by reducing coupling

## The Big Idea

**Objects solve problems by:**
1. **Knowing things** (having state/data)
2. **Doing things** (having behavior/methods)
3. **Asking other objects for help** (collaboration)

## Why Relationships Matter

- Real-world problems are inherently connected
- Software models reality
- Poor relationships cause fragile, unmaintainable systems
- Good relationships lead to flexible, robust designs

### Real-World Examples

Objects don't exist in isolation:
- A `Task` belongs to a `TaskList`
- A `Student` enrolls in `Courses`
- An `Order` contains `Items`

## What Is a Relationship?

A relationship describes:
- **How objects are connected**
- **Who knows about whom**
- **Who depends on whom**

Understanding these connections is crucial for designing maintainable systems.

## Types of Relationships

There are three main types of relationships we focus on:
1. **Association**
2. **Aggregation**
3. **Composition**

### 1. Association

**Definition**: One object knows about another, but there's no ownership implied.

**Characteristics:**
- One object has a reference to another
- No ownership relationship
- Lifetimes are independent
- Objects can exist separately

**Example:**

```java
public class Task {
    private User assignedTo;
}

public class User {
    private List<Task> tasks;
}
```

In this example:
- A `Task` knows about a `User` (who it's assigned to)
- A `User` knows about their `Tasks`
- Neither "owns" the other
- If a User is deleted, Tasks can still exist (and vice versa)

**UML Representation**: Simple line connecting classes

### 2. Aggregation

**Definition**: A whole-part relationship where parts can exist independently.

**Characteristics:**
- Represents a "has-a" relationship
- Parts can exist independently of the whole
- Looser ownership
- Shared lifecycle is possible

**Example:**

```java
public class Team {
    private List<Player> players;
}

public class Player {
    private String id;
    private String name;
    private int age;
    
    public Player(String id, String name, int age) {...}
    
    public String getId() {...}
    public String getName() {...}
    public int getAge() {...}
    public void setName(String name) {...}
    public void setAge(int age) {...}
}
```

In this example:
- A `Team` has `Players`
- Players can exist without a Team
- Players can be on multiple teams
- If a Team is disbanded, Players continue to exist

**UML Representation**: Hollow (empty) diamond on the "whole" side

### 3. Composition

**Definition**: A strong ownership relationship where parts cannot exist independently.

**Characteristics:**
- Strong "has-a" relationship
- Parts cannot exist independently of the whole
- Lifetimes are tightly coupled
- When the whole is destroyed, parts are destroyed too

**Example:**

```java
public class TaskList {
    private List<Task> tasks = new ArrayList<>();
    
    public void addTask(String title) {
        tasks.add(new Task(title));
    }
    
    public void removeTask(Task task) {
        tasks.remove(task);
    }
    
    public List<Task> getTasks() {
        return List.copyOf(tasks);
    }
}

public class Task {
    private String title;
    
    public Task(String title) {...}
    public String getTitle() {...}
}
```

In this example:
- A `TaskList` owns its `Tasks`
- Tasks are created by the TaskList
- Tasks don't exist independently
- When TaskList is destroyed, its Tasks are also destroyed

**UML Representation**: Filled (solid) diamond on the "whole" side

## Choosing the Right Relationship

When designing relationships, ask yourself:

1. **Who owns whom?**
   - Does one object control the other's lifecycle?

2. **Who creates whom?**
   - Which object is responsible for instantiation?

3. **Who controls lifetime?**
   - Can the objects exist independently?

### Decision Framework:

- **Use Association** when: Objects need to know about each other but neither owns the other
- **Use Aggregation** when: There's a whole-part relationship but parts can exist independently
- **Use Composition** when: Parts are integral to the whole and cannot exist independently

## UML Visual Reminder

Understanding UML notation for relationships:

- **Association**: Simple line (→)
- **Aggregation**: Hollow diamond (◇—)
- **Composition**: Filled diamond (◆—)

## Checkpoint Exercise

**Question**: Is this composition or aggregation?

1. **A Playlist and Songs**
   - **Answer**: Aggregation
   - Songs exist independently of playlists
   - Multiple playlists can reference the same song
   - Deleting a playlist doesn't delete the songs

2. **A Human and a Heart**
   - **Answer**: Composition
   - A heart cannot exist independently of a human (in normal circumstances)
   - The heart's lifecycle is tied to the human
   - When the human ceases to exist, so does their specific heart

## Common Relationship Mistakes

Avoid these pitfalls:

1. **Everything knows everything**
   - Creates tight coupling
   - Makes changes ripple through the system
   - Violates encapsulation

2. **Bidirectional relationships everywhere**
   - Adds unnecessary complexity
   - Creates circular dependencies
   - Makes code harder to understand

3. **Premature complexity**
   - Adding relationships "just in case"
   - Over-engineering simple designs
   - Start simple, refactor when needed

### Best Practices:

- Keep relationships as simple as possible
- Prefer unidirectional relationships
- Only add relationships when truly needed
- Design before you code

---

# Object Collaboration and the Law of Demeter

## What Is Object Collaboration?

**Object collaboration** is how objects work together to accomplish tasks.

Objects collaborate by:
- **Calling methods** on other objects
- **Passing messages** between objects
- **Delegating work** to appropriate objects

## Message Passing

In OOP, we think of method calls as "sending messages":
- The calling object sends a message
- The receiving object responds by executing a method
- This is the fundamental mechanism of collaboration

## Collaboration Example

```java
public class TaskList {
    private List<Task> tasks = new ArrayList<>();
    
    public void addTask(String title) {
        tasks.add(new Task(title));
    }
    
    public void removeTask(Task task) {
        tasks.remove(task);
    }
    
    public List<Task> getTasks() {
        return List.copyOf(tasks);
    }
}
```

**TaskList is responsible for managing tasks.**
- It creates tasks
- It stores tasks
- It controls access to tasks
- It encapsulates the collection implementation

## Who Should Do the Work?

When designing collaboration, assign responsibilities to:
- **The object with the most information** about the task
- **The object responsible for the data** being manipulated

This is called the **Information Expert** principle from GRASP (General Responsibility Assignment Software Patterns).

## Bad Collaboration Example

```java
task.getTaskList().getTasks().add(task);
```

**What's wrong with this code?**
- Too much chaining (method chaining)
- The caller needs to know too much about internal structure
- Breaks encapsulation
- Creates tight coupling
- Violates the Law of Demeter

### Problems:
1. **High coupling**: The code depends on TaskList's internal structure
2. **Breaks encapsulation**: Exposes internal implementation details
3. **Fragile design**: Changes to TaskList break this code

## The Law of Demeter

Also known as the **"Principle of Least Knowledge"** or **"Don't Talk to Strangers"**

### The Rule:

**An object should only talk to:**
1. **Itself** (its own methods)
2. **Its fields** (objects it has as instance variables)
3. **Its method parameters** (objects passed to it)
4. **Objects it creates** (objects instantiated within its methods)

**It should NOT talk to:**
- Objects returned by its methods (beyond the immediate return)
- Objects returned by methods of its fields

### Purpose:

This helps:
- Reduce coupling between classes
- Increase maintainability
- Make code more flexible and easier to change
- Protect encapsulation

### Metaphor: Only Talk to Immediate Friends

"A given object should only talk to its immediate friends, not strangers."

Think of it like social etiquette:
- You can talk to your friends directly
- You shouldn't ask your friend to get their friend to get their friend to do something
- Keep communication direct and simple

## Origin: The Demeter Project

**Historical Context:**

The Law of Demeter is named after the **Demeter Project** at Northeastern University in the late 1980s.

### Why "Demeter"?

- **Demeter** is the Greek goddess of agriculture and harvest
- The project was about **adaptive programming** and **simplifying object interactions**
- Metaphorically aimed to "cultivate" better software design
- Software should "grow" in a controlled, healthy way
- Just as Demeter nurtured crops, the project aimed to nurture clean code
- Prevent "weeds" (bad dependencies) from overrunning the codebase

## Violates Law of Demeter

**Example of violation (talking to strangers):**

```java
// Too much "chaining", talking to strangers
double price = order.getCustomer().getAddress().getZipCode().getTaxRate();
```

**Why this is bad:**
- Chains through multiple objects
- Depends on internal structure of Customer, Address, and ZipCode
- If any intermediate structure changes, this code breaks
- Creates tight coupling across many classes

**Problems:**
1. High coupling to multiple classes
2. Knowledge of deep object structure
3. Fragile to changes anywhere in the chain
4. Hard to test and maintain

## Follows Law of Demeter

**Better approach (asking the immediate friend):**

```java
// Ask the object directly, only talk to immediate friends
double price = order.getTaxRate();
```

**Why this is better:**
- `order` is the immediate object we have
- We ask it directly for what we need
- Internal structure is hidden
- Order can change how it calculates tax rate without affecting callers
- Low coupling, high encapsulation

**Benefits:**
1. Low coupling
2. Hidden implementation details
3. Flexible to change
4. Easy to test

## Encapsulation Reinforced

The Law of Demeter reinforces good encapsulation:

1. **Hide internal structure**
   - Don't expose how objects are organized internally
   - Provide methods that abstract away details

2. **Expose meaningful behavior**
   - Offer high-level operations
   - Hide low-level implementation

3. **Reduce knowledge leaks**
   - Minimize what callers need to know
   - Keep interfaces simple and focused

## Coupling

**Definition**: The degree of dependency between classes.

### High Coupling (Bad):
- Classes depend on many other classes
- Changes ripple through the system
- Hard to test in isolation
- Fragile and risky to modify
- Reduces reusability

### Low Coupling (Good):
- Classes have minimal dependencies
- Changes are localized
- Easy to test
- Resilient to change
- Promotes reusability

**Goal**: Aim for low coupling between classes.

## Cohesion

**Definition**: How focused and purposeful a class is.

### Low Cohesion (Bad):
- Class does many unrelated things
- Multiple responsibilities
- Hard to understand and maintain
- Difficult to reuse

### High Cohesion (Good):
- Class has one clear purpose
- All methods relate to that purpose
- Easy to understand
- Easy to maintain and test
- Highly reusable

**Goal**: Aim for high cohesion within classes.

## Balance Matters

**The ideal design has:**
1. **Low coupling** (between classes)
2. **High cohesion** (within classes)
3. **Clear responsibilities** (each class knows its job)

These three principles work together to create maintainable, flexible software.

## Collaboration Refactoring

When you find violations of good collaboration, refactor:

### Strategies:

1. **Move logic closer to data**
   - Put behavior in the class that owns the data
   - Follow the Information Expert principle

2. **Replace chains with methods**
   - Instead of: `a.getB().getC().doSomething()`
   - Use: `a.doSomething()`

3. **Let objects do their job**
   - Don't micromanage objects from the outside
   - Ask objects to perform operations, don't extract data and do it yourself

### Example Refactoring:

**Before (Bad):**
```java
if (task.getCompleted() == false) {
    task.setCompleted(true);
    task.setCompletedDate(new Date());
}
```

**After (Good):**
```java
task.markCompleted();
```

## Fun Rule of Thumb

**"If your code reads like a sentence with many dots, it might be gossiping too much."**

Count the dots (method calls):
- `object.method()` - One dot, usually fine
- `object.method1().method2()` - Two dots, be cautious
- `object.method1().method2().method3()` - Three+ dots, likely violating Law of Demeter

**Exception**: Fluent interfaces and builders intentionally use chaining, but they return the same object or build a single object, not navigating through different objects' structures.

---

# Designing Collaboration in Practice

## Starting Design

Let's work with three classes:
- **Task**: Stores task data
- **TaskList**: Manages a collection of tasks
- **TaskManager**: Coordinates actions on tasks

## Initial Responsibilities

Clear responsibility assignment:

1. **Task**
   - Stores task data (description, completion status)
   - Knows how to change its own state

2. **TaskList**
   - Manages a collection of tasks
   - Adds and removes tasks
   - Provides access to tasks

3. **TaskManager**
   - Coordinates high-level operations
   - Delegates work to TaskList and Task
   - Provides the main interface for users

## Initial Collaboration Scenario

**Requirement**: TaskManager should be able to complete a task.

```java
taskManager.completeTask(taskId);
```

**Question**: How should this be implemented?

## Who Should Complete a Task?

Let's consider three options:

### Option 1: TaskManager Does It
```java
public class TaskManager {
    public void completeTask(int taskId) {
        Task task = taskList.getTaskById(taskId);
        task.completed = true; // BAD: Accessing field directly
    }
}
```

**Problems:**
- TaskManager manipulates Task's internal state
- Breaks encapsulation
- TaskManager knows too much about Task

### Option 2: TaskList Does It
```java
public class TaskList {
    public void completeTask(int taskId) {
        Task task = getTaskById(taskId);
        task.completed = true; // BAD: Still breaks encapsulation
    }
}
```

**Problems:**
- TaskList manipulates Task's state
- Task is just a data holder with no behavior
- Still violates encapsulation

### Option 3: Task Does It (Best)
```java
public class Task {
    private boolean completed;
    
    public void markCompleted() {
        this.completed = true;
    }
}

public class TaskManager {
    public void completeTask(int taskId) {
        Task task = taskList.getTaskById(taskId);
        task.markCompleted(); // GOOD: Delegating to Task
    }
}
```

**Why this is best:**
- Task owns its state
- Task provides behavior to modify its state
- TaskManager delegates to Task
- Good encapsulation

## Refactored Design

**Final design principle**: **Behavior lives with the data.**

```java
task.markCompleted();
```

This simple call demonstrates:
- Clear responsibility
- Good encapsulation
- Low coupling
- High cohesion

## Responsibility Shift

### Before Refactoring:
- **Centralized logic**: TaskManager or TaskList did everything
- **Anemic domain model**: Task was just a data holder
- **High coupling**: Manager/List knew about Task internals

### After Refactoring:
- **Distributed responsibilities**: Each class does its own job
- **Rich domain model**: Task has meaningful behavior
- **Low coupling**: Objects respect each other's boundaries

## UML Snapshot

Final design showing relationships:

```
┌─────────────────────┐
│   TaskManager       │
│                     │
│ -taskList: TaskList │
│ +completeTask(int)  │
└──────────┬──────────┘
           │ delegates
           ▼
┌─────────────────────┐
│   TaskList          │
│                     │
│ -tasks: List<Task>  │
│ +addTask(Task)      │
│ +getTaskById(int)   │
└──────────┬──────────┘
           │ composes
           ▼ 1
┌─────────────────────┐
│   Task              │
│                     │
│ -description: String│
│ -completed: boolean │
│ +markCompleted()    │
│ +isCompleted()      │
└─────────────────────┘
```

**Key points:**
- TaskList **composes** Task (strong ownership)
- Task **exposes behavior** (not just getters/setters)
- TaskManager **delegates** (coordinates without micromanaging)

## Collaboration Smells

Watch out for these code smells indicating poor collaboration:

### 1. Long Method Chains
```java
// BAD
user.getAccount().getProfile().getAddress().getZipCode();
```

**Solution**: Provide direct methods
```java
// GOOD
user.getZipCode();
```

### 2. God Objects
A class that knows too much and does too much.

**Symptoms:**
- Hundreds of methods
- Knows about many other classes
- Central point of control

**Solution**: Break into smaller, focused classes

### 3. Objects Doing Others' Jobs
```java
// BAD: Manager manipulating Task's internals
task.setCompleted(true);
task.setCompletedDate(new Date());
task.notifyObservers();
```

**Solution**: Let objects manage themselves
```java
// GOOD
task.markCompleted();
```

## Refactoring Checklist

When reviewing collaboration, ask:

1. **Can I move this method?**
   - Is the method manipulating data it doesn't own?
   - Would it make more sense in another class?

2. **Does this object know too much?**
   - Is it aware of internal structure of many classes?
   - Smells like "God Class"?

3. **Is this relationship necessary?**
   - Can I accomplish the same thing with less coupling?
   - Am I adding relationships "just in case"?

### Red Flags:
- Multiple dots in a single statement
- Classes with generic names (Manager, Helper, Util)
- Classes that import many other classes
- Methods that accept and return many parameters

## What You Just Learned

Key takeaways from this section:

1. **Relationships shape design**
   - How you connect classes affects the entire system
   - Choose relationships carefully

2. **Collaboration drives behavior**
   - Objects working together is how tasks get done
   - Good collaboration means asking, not commanding

3. **Encapsulation is enforced through structure**
   - Proper relationships protect internal details
   - Law of Demeter helps maintain boundaries

## Summary: Object Relationships and Collaboration

### Relationships:
- **Association**: Objects know about each other
- **Aggregation**: Whole-part, parts independent
- **Composition**: Whole-part, parts dependent

### Collaboration:
- Objects work together through messages
- Follow the Law of Demeter
- Keep coupling low, cohesion high

### Design Principles:
- Put behavior with data
- Hide internal structure
- Let objects do their jobs
- Reduce coupling

## Up Next!

Topics to be covered:
- **Inheritance**: Extending classes to create hierarchies
- **Interfaces**: Defining contracts for behavior
- **Polymorphism**: One interface, many implementations
- **When not to inherit**: Avoiding inheritance pitfalls