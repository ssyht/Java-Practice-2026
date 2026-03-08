# L7 – Introduction to Unit Testing and JUnit 5
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe | Mizzou**

---

## Table of Contents
1. [Why Testing Matters](#1-why-testing-matters)
2. [Types of Testing](#2-types-of-testing)
3. [JUnit 5 Architecture & Project Structure](#3-junit-5-architecture--project-structure)
4. [Writing Basic Tests](#4-writing-basic-tests)
5. [Assertions Reference](#5-assertions-reference)
6. [The AAA Pattern (Arrange–Act–Assert)](#6-the-aaa-pattern-arrangeactassert)
7. [Boundary Value Analysis](#7-boundary-value-analysis)
8. [Parameterized Testing](#8-parameterized-testing)
9. [Setup and Teardown](#9-setup-and-teardown)
10. [Testing Generic Classes](#10-testing-generic-classes)
11. [Good Test Design](#11-good-test-design)
12. [Mutation Testing with PIT](#12-mutation-testing-with-pit)
13. [Regression Testing](#13-regression-testing)
14. [Quick-Reference Cheat Sheet](#14-quick-reference-cheat-sheet)

---

## 1. Why Testing Matters

Testing is not optional — it's a core part of writing maintainable, professional code.

| Benefit | What it means |
|---|---|
| Catch bugs early | Find problems before they reach production |
| Prevent regressions | Make sure old behavior isn't broken by new code |
| Improve design | Testable code tends to be better-designed code |
| Document behavior | Tests show exactly what the code is supposed to do |

---

## 2. Types of Testing

```
Unit Testing        → Test a single method or class in isolation
Integration Testing → Test how multiple components work together
System Testing      → Test the full application end-to-end
Acceptance Testing  → Verify the system meets business requirements
```

**Unit testing** is what we focus on in L7. A unit test targets:
- A single method
- A single class
- A small, isolated component

---

## 3. JUnit 5 Architecture & Project Structure

JUnit 5 is composed of three layers:
- **JUnit Platform** – foundation for running tests on the JVM
- **JUnit Jupiter** – new programming model and annotations (what you write)
- **JUnit Vintage** – backward compatibility with JUnit 3/4

### Maven Project Structure

```
src/
├── main/
│   └── java/          ← production source code
└── test/
    └── java/          ← test source code
```

### Naming Convention

| Production class | Test class |
|---|---|
| `TaskService.java` | `TaskServiceTest.java` |

Test method names use descriptive `should...` style:
```java
shouldAddTask()
shouldRejectNullTask()
```

---

## 4. Writing Basic Tests

Every test method is annotated with `@Test`.

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class TaskListTest {

    @Test
    public void shouldAddTask() {
        TaskList list = new TaskList();
        list.addTask(new Task("Study"));

        assertEquals(1, list.size());
    }
}
```

> **Key rule:** Each test method should test **one behavior**. If a test has multiple unrelated assertions, split it into separate tests.

---

## 5. Assertions Reference

All assertion methods live in `org.junit.jupiter.api.Assertions`.

| Method | Purpose |
|---|---|
| `assertEquals(expected, actual)` | Values are equal |
| `assertNotEquals(unexpected, actual)` | Values are NOT equal |
| `assertTrue(condition)` | Condition is true |
| `assertFalse(condition)` | Condition is false |
| `assertNull(object)` | Object is null |
| `assertNotNull(object)` | Object is not null |
| `assertThrows(ExceptionType.class, lambda)` | Code throws expected exception |

### Testing Exceptions with `assertThrows`

```java
@Test
public void shouldRejectEmptyDescription() {
    assertThrows(
        IllegalArgumentException.class,
        () -> new Task("")
    );
}
```

The lambda `() -> new Task("")` is the code that **should** throw. If it doesn't throw (or throws the wrong exception), the test fails.

---

## 6. The AAA Pattern (Arrange–Act–Assert)

Every well-structured unit test has three sections:

```
Arrange → Set up everything needed (objects, data, inputs)
Act     → Call the one method being tested
Assert  → Verify the result is correct
```

### Example

```java
@Test
public void shouldAddTask() {

    // Arrange
    TaskList list = new TaskList();
    Task task = new Task("Study");

    // Act
    list.addTask(task);

    // Assert
    assertEquals(1, list.size());
}
```

### Bad vs. Better Test

**Bad** – multiple actions and multiple unrelated assertions:
```java
@Test
public void testEverything() {
    TaskList list = new TaskList();
    list.addTask(new Task("A"));
    list.addTask(new Task("B"));
    assertEquals(2, list.size());
    list.removeTask("A");
    assertEquals(1, list.size());   // Too many behaviors in one test!
}
```

**Better** – one behavior per test:
```java
@Test
public void shouldAddTask() { ... }

@Test
public void shouldRemoveTask() { ... }
```

> A good test usually has **only one Act step**.

---

## 7. Boundary Value Analysis

Bugs cluster at the **edges** of valid input ranges. Always test:
- The minimum valid value
- The maximum valid value
- One step below the minimum (should fail)
- One step above the maximum (should fail)

### Example

If a `Task` description must be between 1 and 100 characters, test: `0, 1, 100, 101`.

```java
@Test
public void descriptionTooShort() {
    assertThrows(IllegalArgumentException.class,
        () -> new Task(""));   // length = 0, just below minimum
}

@Test
public void descriptionAtMinimum() {
    assertNotNull(new Task("A"));   // length = 1, valid minimum
}

@Test
public void descriptionAtMaximum() {
    assertNotNull(new Task("A".repeat(100)));   // length = 100, valid maximum
}

@Test
public void descriptionTooLong() {
    assertThrows(IllegalArgumentException.class,
        () -> new Task("A".repeat(101)));   // length = 101, just above maximum
}
```

---

## 8. Parameterized Testing

**Motivation:** Without parameterized tests you write nearly identical test methods for every input value. Parameterized tests let you run the same test logic against multiple inputs automatically.

### `@ValueSource` — simple inline values

**Strings:**
```java
@ParameterizedTest
@ValueSource(strings = {"A", "Task", "Study"})
public void validDescriptions(String input) {
    Task task = new Task(input);
    assertNotNull(task);
}
```

**Ints:**
```java
@ParameterizedTest
@ValueSource(ints = {0, 1, 10})
public void testValues(int value) {
    assertTrue(value >= 0);
}
```

Each value in the array is injected as a separate test run.

### `@MethodSource` — complex objects

Use `@MethodSource` when your test needs full objects or multiple parameters. Point it at a static factory method that returns a `Stream<Arguments>`.

**Factory method:**
```java
public static Stream<Arguments> taskCases() {
    return Stream.of(
        Arguments.of(new Task("Study"), true),
        Arguments.of(new Task("Sleep"), true)
    );
}
```

**Test method:**
```java
@ParameterizedTest
@MethodSource("taskCases")
public void testTaskCompletion(Task task, boolean expected) {
    task.complete();
    assertEquals(expected, task.isCompleted());
}
```

### Benefits of Parameterized Tests

- Reduces duplication
- Improves coverage with minimal extra code
- Easy to extend — just add more values to the source

---

## 9. Setup and Teardown

Use lifecycle annotations to run code **before or after** tests without repeating setup in every method.

| Annotation | When it runs |
|---|---|
| `@BeforeEach` | Before **each** test method |
| `@AfterEach` | After **each** test method |
| `@BeforeAll` | Once before **all** tests in the class (must be `static`) |
| `@AfterAll` | Once after **all** tests in the class (must be `static`) |

### Example

```java
class TaskListTest {

    private TaskList taskList;

    @BeforeEach
    public void setup() {
        taskList = new TaskList();   // fresh instance before every test
    }

    @Test
    public void shouldAddTask() {
        taskList.addTask(new Task("Study"));
        assertEquals(1, taskList.size());
    }
}
```

> `@BeforeEach` guarantees test **independence** — no test can accidentally affect another.

---

## 10. Testing Generic Classes

Generic classes are tested by instantiating them with **concrete types**. There is nothing special about the test itself — just pick a type.

**The generic class under test:**
```java
public class Box<T> {
    private T item;

    public void set(T item) { this.item = item; }
    public T get()           { return item; }
}
```

**Test with an object type (`Task`):**
```java
@Test
public void boxShouldStoreTask() {
    Box<Task> box = new Box<>();
    Task task = new Task("Study");

    box.set(task);

    assertEquals(task, box.get());
}
```

**Test with a primitive wrapper (`String`):**
```java
@Test
public void boxShouldStoreString() {
    Box<String> box = new Box<>();
    box.set("Hello");
    assertEquals("Hello", box.get());
}
```

> Test generics with **multiple concrete types** to ensure the class truly works generically.

---

## 11. Good Test Design

### Characteristics of a good test

| Property | Meaning |
|---|---|
| **Readable** | Anyone can understand what the test checks |
| **Independent** | Tests don't depend on each other's state |
| **Deterministic** | Same result every time you run it |
| **Small** | Tests one thing only |

### Naming best practices

Use behavior-describing method names:
```java
shouldCompleteTask()
shouldRejectInvalidTask()
shouldReturnNullWhenEmpty()
```

Avoid generic names like `test1()` or `testTask()`.

---

## 12. Mutation Testing with PIT

### The problem with line coverage

Traditional code coverage only tells you whether a line **ran**. It doesn't tell you whether your assertions would actually **catch a bug**.

### What mutation testing does

1. PIT automatically modifies ("mutates") your source code — e.g., changes `>` to `>=`, flips a boolean, removes a return value
2. It re-runs your test suite against each mutant
3. If your tests **still pass** after the mutation → the tests are **weak** (they missed the bug)
4. If your tests **fail** → the mutation was "killed" (tests caught it) ✅

### Example

```java
// Original code
if (x > 10) { ... }

// Mutant generated by PIT
if (x >= 10) { ... }   // If tests still pass, you have a gap!
```

### Tool

**PIT** (https://pitest.org) integrates with Maven and has an Eclipse plugin. It reports a **mutation score** — the percentage of mutants killed by your tests.

---

## 13. Regression Testing

Running **all existing tests** after making a change to ensure nothing that worked before is now broken.

- Very common and valuable in professional software development
- In large codebases, regression test suites can take **days to weeks** to run completely
- JUnit makes regression testing automatic — just re-run the suite

---

## 14. Quick-Reference Cheat Sheet

### Annotations

```
@Test                         Mark a method as a test
@ParameterizedTest            Mark a method as parameterized
@ValueSource(strings={...})   Inject inline string/int/etc. values
@ValueSource(ints={...})
@MethodSource("methodName")   Inject values from a factory method
@BeforeEach                   Run before each test
@AfterEach                    Run after each test
@BeforeAll                    Run once before all tests (static)
@AfterAll                     Run once after all tests (static)
```

### Assertions

```java
assertEquals(expected, actual)
assertNotEquals(unexpected, actual)
assertTrue(condition)
assertFalse(condition)
assertNull(obj)
assertNotNull(obj)
assertThrows(Exception.class, () -> { /* code */ })
```

### Test structure template

```java
@Test
public void shouldDoSomething() {
    // Arrange
    MyClass obj = new MyClass();

    // Act
    int result = obj.doSomething();

    // Assert
    assertEquals(42, result);
}
```

### Boundary value checklist

```
Given valid range [min, max], always test:
  min - 1   → should throw / fail
  min       → valid
  max       → valid
  max + 1   → should throw / fail
```

### Maven project structure

```
src/
├── main/java/   ← production code (TaskList.java, Task.java, ...)
└── test/java/   ← test code       (TaskListTest.java, ...)
```