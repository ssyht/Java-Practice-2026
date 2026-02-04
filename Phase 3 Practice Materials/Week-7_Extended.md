# Week Notes: Class Invariants, Mutability, and Immutability

## Class Invariants

**Class invariants** are conditions that must always hold true for an object throughout its lifetime.

- These conditions define the valid states an object can be in
- Enforced by constructors and methods
- Help maintain object consistency and correctness
- Prevent invalid states that could lead to bugs

## Mutability

**Mutable objects** are objects whose state can change after creation.

### Characteristics:
- State changes over time
- Useful for modeling real-world entities that change
- Can be risky if not properly controlled
- **Note**: Mutability is not inherently bad, but it must be carefully managed

### Trade-offs:
- More flexible for certain use cases
- Can lead to unexpected side effects
- Harder to reason about in concurrent environments
- Requires careful design to maintain invariants

## Immutability

**Immutable objects** are objects whose state never changes after construction.

### Characteristics:
- Once created, the object's state cannot be modified
- Changes are represented by creating new objects
- Easier to reason about and debug
- Thread-safe by default

### Benefits of Immutability:
- **Thread safe**: No synchronization needed in concurrent environments
- **Easier testing**: Predictable behavior without hidden state changes
- **No hidden side effects**: Methods don't unexpectedly modify state
- Simplifies debugging and maintenance

## The `final` Modifier in Java

The `final` keyword can be applied to different Java elements with different meanings:

### Applications of `final`:
- Fields
- Local variables
- Method parameters
- Methods (covered later)
- Classes (covered later)

The meaning depends on context, but it always communicates intent to readers.

### `final` Fields

A `final` field has the following properties:
- Must be assigned exactly once
- Cannot be reassigned afterward
- Usually set in a constructor
- Key building block for creating immutable classes

**Example**:
```java
public class Money {
    private final int cents;
    
    public Money(int cents) {
        this.cents = cents;
    }
    
    // cents cannot be reassigned after construction
}
```

### `final` References vs. Object State

**Important distinction**: `final` makes the *reference* immutable, not necessarily the object itself.

```java
final List<String> names = new ArrayList<>();
names.add("Alice");  // ✓ Allowed - modifying object state
// names = new ArrayList<>();  // ✗ Not allowed - reassigning reference
```

- The reference `names` cannot be reassigned
- But the `ArrayList` object itself can still be modified
- To achieve true immutability, the referenced object must also be immutable

### `final` Parameters and Local Variables

Using `final` with parameters and local variables:
- Prevents accidental reassignment
- Improves code readability
- Useful in constructors and methods to ensure values don't change
- Makes intent clear to other developers

**Example**:
```java
public void addTask(final Task task) {
    // task cannot be reassigned
    // Prevents bugs like: task = someOtherTask;
}
```

### Why Use `final`?

- **Enforces invariants**: Helps maintain object consistency
- **Reduces bugs**: Prevents accidental reassignment
- **Makes code easier to reason about**: Clear which values can change
- **Supports immutable design**: Foundation for immutable classes

### What We're Not Covering Yet

The following uses of `final` will be covered when we discuss inheritance:
- `final` methods
- `final` classes

## Java Records

**Records** are a special kind of class introduced in modern Java for creating simple data carriers.

### Characteristics:
- Compact syntax
- Immutable by default
- Automatically generates:
  - Constructor
  - Getters
  - `equals()` and `hashCode()`
  - `toString()`
- Ideal for data carriers and DTOs (Data Transfer Objects)

### Record Example

```java
public record Task(int value) {
    // Canonical constructor
    public Task {
        if (value < 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

This automatically provides:
- `int value()` accessor method
- Constructor with validation
- Proper `equals()`, `hashCode()`, and `toString()`

### When NOT to Use Records

Records are not appropriate for:
- Classes with complex behavior
- Mutable state requirements
- Rich domain logic
- Cases needing inheritance (records are final)

**Remember**: Records are not a replacement for all classes! They're specifically for simple, immutable data carriers.

## Common Constructor Mistakes

Avoid these common pitfalls when designing constructors:

1. **Too many parameters**: Makes the constructor hard to use and understand
2. **No validation**: Allows invalid objects to be created
3. **Doing too much work**: Constructors should primarily initialize state, not perform complex operations

**Note**: Builder patterns (covered later) can help address some of these issues.

## Refactoring Classes: Applying Design Choices

### Focus Areas:
- Apply encapsulation principles
- Use constructors properly
- Practice immutability where appropriate

### Starting Point: Poor Design

A Task class with public fields and no validation:

```java
class Task {
    public String description;
    public boolean completed;
}

// Procedural usage:
Task task = new Task();
task.description = "";  // Invalid state!
task.completed = false;
```

### Step 1: Identify Invariants

Define what must always be true:
- Description cannot be empty
- Completion status starts as `false`

### Step 2: Add Constructor

```java
public class Task {
    public String description;
    public boolean completed;
    
    public Task(String description) {
        if (description.isBlank()) {
            throw new IllegalArgumentException();
        }
        this.description = description;
        this.completed = false;
    }
}
```

### Step 3: Encapsulate State

Make fields private and add behavior methods:

```java
public class Task {
    private String description;
    private boolean completed;
    
    public Task(String description) {
        if (description.isBlank()) {
            throw new IllegalArgumentException();
        }
        this.description = description;
        this.completed = false;
    }
    
    public void markCompleted() {
        this.completed = true;
    }
    
    public boolean isCompleted() {
        return completed;
    }
    
    public String getDescription() {
        return description;
    }
}
```

### Step 4: Improve Naming

Use intention-revealing names:
- `markCompleted()` instead of `setCompleted(boolean)`
- `renameDescription(String)` instead of `setDescription(String)`

This makes the API more expressive and harder to misuse.

### Design Decision: Mutable or Immutable?

**Question to consider**: Should Task be mutable?

**Considerations**:
- If tasks need to change state (e.g., marking as completed), mutability makes sense
- If tasks should be immutable, create new Task objects to represent state changes
- Consider your use case and threading requirements

### Refactoring Result

The refactored class has:
- Clear responsibilities
- Safer object creation and modification
- Better overall design
- Protected invariants

### UML Updated View

The improved design shows:
- Task with a proper constructor
- Private fields
- Public behavior methods
- Clear encapsulation boundaries

## Common Student Questions

### Do we always avoid setters?
Not necessarily. Avoid "dumb" setters that just assign values without validation or logic. Prefer methods that express behavior and intent.

### Are records always better?
No. Records are great for simple data carriers, but regular classes are needed for complex behavior, mutable state, or rich domain logic.

### How much validation is enough?
Validate enough to ensure your invariants hold. Consider what states would lead to bugs or incorrect behavior, and prevent those in your constructor.

## Key Takeaways

1. **Classes protect their state**: Use encapsulation to maintain control
2. **Constructors enforce correctness**: Validate inputs and establish invariants
3. **Immutability is powerful**: Consider immutable designs when appropriate
4. Use `final` to communicate intent and prevent bugs
5. Records are useful for simple data carriers, but not a universal solution

## Up Next

Topics to be covered:
- **Object relationships**: How objects work together
- **Collaboration**: Objects communicating and cooperating
- **Composition vs. Aggregation**: Different types of object relationships