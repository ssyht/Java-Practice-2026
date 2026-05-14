Here are the markdown notes for L15:

```markdown
# L15 – Exception Handling
**CMP_SC 3330 – Object-Oriented Programming**  
Dr. Ekincan Ufuktepe | University of Missouri – Columbia

---

## What is an Exception?

An exception is:
- An abnormal condition
- That disrupts program flow
- Represented as an object

### Why Exceptions Exist
- Separate error handling from logic
- Avoid silent failures
- Provide useful information

---

## Exception Hierarchy

```
Throwable
├── Error             (serious system issues — don't catch)
└── Exception         (recoverable problems)
    └── RuntimeException
```

- **Throwable** → root of all exceptions
- **Error** → serious system issues (e.g., `OutOfMemoryError`) — not meant to be caught
- **Exception** → recoverable problems you should handle
- **RuntimeException** → subclass of Exception; unchecked

---

## Checked vs. Unchecked Exceptions

| | Checked | Unchecked |
|---|---|---|
| When checked | Compile time | Runtime |
| Must handle? | Yes | No |
| Extends | `Exception` | `RuntimeException` |
| Example | `IOException` | `NullPointerException`, `IllegalArgumentException` |

---

## try–catch Basics

```java
try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}
```

### Multiple Catch Blocks

```java
try {
    // code
} catch (IOException e) {
    // handle IO
} catch (Exception e) {
    // handle anything else
}
```

> Catch more specific exceptions **first**, more general ones **last**.

### finally Block

```java
finally {
    System.out.println("Always runs");
}
```

The `finally` block **always executes**, whether or not an exception occurred. Used for cleanup (e.g., closing files).

---

## try-with-resources

Automatically closes resources (implements `AutoCloseable`) when the block exits.

```java
try (BufferedReader br = new BufferedReader(...)) {
    // use br
}
// br is automatically closed here
```

---

## Designing with Exceptions – Best Practices

### Fail-Fast Principle
- Detect problems **early**
- Stop execution **immediately** when something is wrong

```java
if (desc == null || desc.isBlank()) {
    throw new IllegalArgumentException("Invalid description");
}
```

**Why fail fast?**
- Bugs are found early
- Simpler code later
- No invalid objects can exist

### When TO Throw Exceptions
- Invalid input
- Invalid state
- Impossible conditions

### When NOT to Use Exceptions
- Normal flow control
- Expected behavior (use `if` checks instead)

### Bad Example – Swallowing an Exception
```java
try {
    list.get(10);
} catch (Exception e) {
    // ignore  ← NEVER do this
}
```

### Good Design
Check conditions instead of catching everything:
```java
if (index < list.size()) {
    list.get(index);
}
```

### Exception Messages Matter
```java
throw new IllegalArgumentException("Task description cannot be empty");
```

### Catching Too Broadly
| Bad | Better |
|---|---|
| `catch (Exception e)` | `catch (IOException e)` |

### Rethrowing Exceptions
Convert a checked exception to unchecked when you can't handle it locally:
```java
catch (IOException e) {
    throw new RuntimeException(e);
}
```

---

## Custom Exceptions

### Why Custom Exceptions?
- Express **domain meaning** (e.g., `TaskNotFoundException` vs. generic `RuntimeException`)
- Improve readability
- Enable better error handling

### Creating a Checked Custom Exception
```java
public class InvalidTaskException extends Exception {
    public InvalidTaskException(String message) {
        super(message);
    }
}
```

### Creating an Unchecked Custom Exception
```java
public class TaskNotFoundException extends RuntimeException {
    public TaskNotFoundException(String msg) {
        super(msg);
    }
}
```

### Checked vs. Unchecked Custom Exceptions
| | Checked | Unchecked |
|---|---|---|
| Extends | `Exception` | `RuntimeException` |
| Use when | Caller **must** handle it | Programming error |

### Using a Custom Exception
```java
if (desc.isBlank()) {
    throw new InvalidTaskException("Description cannot be empty");
}
```

---

## Anti-Patterns to Avoid

An **anti-pattern** is a repeated bad habit that seems fine at first but causes problems later.

- **Empty catch blocks** (swallowing exceptions) — hides bugs silently
- **Catching `Throwable`** — too broad; catches `Error`s you shouldn't touch
- **Using exceptions for control flow** — use `if` statements instead

---

## Summary

- Exceptions represent abnormal conditions as objects
- Java uses a hierarchy: `Throwable → Error / Exception → RuntimeException`
- **Checked** exceptions must be handled; **unchecked** don't have to be
- Use `try–catch–finally` and `try-with-resources` to handle exceptions safely
- **Fail fast** — validate early, throw immediately
- **Catch specific** exceptions, not broad ones
- **Never swallow** exceptions silently
- **Custom exceptions** add domain meaning and improve code clarity
```