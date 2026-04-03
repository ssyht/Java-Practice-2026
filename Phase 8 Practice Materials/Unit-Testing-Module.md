# L7 – Introduction to Unit Testing and JUnit 5

**CMP_SC 3330 – Object-Oriented Programming**
**Dr. Ekincan Ufuktepe | University of Missouri – Columbia**

---

## 1. Why Testing Matters

Writing code is only half the job. Testing is how you make sure the code actually *works*.

Testing helps you:
- **Catch bugs early** — find problems before they reach production
- **Prevent regressions** — make sure old features don't break when you add new ones
- **Improve design** — well-structured code is easier to test
- **Document behavior** — tests show *how* a class is supposed to behave

---

## 2. Types of Testing

| Type | What it tests |
|------|--------------|
| **Unit Testing** | A single method, class, or small component in isolation |
| **Integration Testing** | How multiple components work together |
| **System Testing** | The entire application end-to-end |
| **Acceptance Testing** | Whether the system meets business requirements |

In this course, we focus on **unit testing**.

---

## 3. What Is Unit Testing?

A unit test verifies the behavior of a **small, isolated piece of code** — typically a single method or class — in isolation from the rest of the system.

Examples of units:
- A method like `addTask()`
- A class like `TaskList`
- A small component

---

## 4. JUnit 5 Architecture

JUnit 5 is the standard Java testing framework. The architecture works like this:

```
TestRunner
    └── TestClass
            └── MethodUnderTest
```

- **TestRunner** discovers and runs your test classes
- **TestClass** groups related test methods (e.g., `TaskServiceTest.java`)
- Each test method directly calls and verifies a **method under test**

### Maven Project Structure

```
src/
├── main/
│   └── java/          ← your production code
└── test/
    └── java/          ← your test classes
```

### Naming Conventions

| Production class | Test class |
|------------------|------------|
| `TaskService.java` | `TaskServiceTest.java` |

Test methods should use descriptive, behavior-focused names:
- `shouldAddTask()`
- `shouldRejectNullTask()`

---

## 5. Writing a Basic JUnit Test

Every test method is annotated with `@Test`:

```java
@Test
public void shouldAddTask() {
    TaskList list = new TaskList();
    list.addTask(new Task("Study"));

    assertEquals(1, list.size());
}
```

### Common Assertions

| Assertion | What it checks |
|-----------|---------------|
| `assertEquals(expected, actual)` | Two values are equal |
| `assertTrue(condition)` | Condition is true |
| `assertFalse(condition)` | Condition is false |
| `assertThrows(ExceptionType.class, () -> ...)` | Code throws the expected exception |
| `assertNotNull(object)` | Object is not null |

### Testing for Exceptions

```java
@Test
public void shouldRejectEmptyDescription() {
    assertThrows(
        IllegalArgumentException.class,
        () -> new Task("")
    );
}
```

The lambda `() -> new Task("")` is the code you expect to throw. If it does, the test passes.

---

## 6. The Arrange–Act–Assert (AAA) Pattern

Every good test follows three clear phases:

| Phase | Purpose |
|-------|---------|
| **Arrange** | Set up the environment — create objects, define inputs |
| **Act** | Execute the behavior being tested (usually one line) |
| **Assert** | Verify the result was what you expected |

Think of it as: **setup → action → verification**

### AAA Example

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

### Why One Action Per Test?

A good test has **only one Act step**. If a test performs many actions, it becomes:
- Harder to understand
- Harder to debug when it fails
- Unclear which behavior it's actually verifying

---

## 7. Bad vs. Good Tests

### ❌ Bad Test

```java
@Test
public void testAddTask() {
    TaskList list = new TaskList();
    list.addTask(new Task("Study"));
    list.addTask(new Task("Sleep"));

    assertEquals(2, list.size());   // assertion 1

    list.removeTask(0);

    assertEquals(1, list.size());   // assertion 2
}
```

**Problems:**
- Multiple actions (add, add, remove)
- Multiple assertions
- Unclear purpose — what behavior is being tested?

### ✅ Better Tests

Split into two focused tests:

```java
@Test
public void shouldAddTask() {
    TaskList list = new TaskList();
    list.addTask(new Task("Study"));
    assertEquals(1, list.size());
}

@Test
public void shouldRemoveTask() {
    TaskList list = new TaskList();
    list.addTask(new Task("Study"));
    list.removeTask(0);
    assertEquals(0, list.size());
}
```

Each test verifies **one behavior**. When a test fails, you know exactly what's broken.

---

## 8. Boundary Value Analysis

Bugs tend to cluster at the **edges** of valid input ranges. Boundary Value Analysis (BVA) means you deliberately test those edge cases.

**Rule:** If valid input is between `min` and `max`, test: `min-1`, `min`, `max`, `max+1`.

**Example:** Valid task description length is between 1 and 100 characters.

| Input length | Expected behavior |
|-------------|-------------------|
| 0 | Throw exception (too short) |
| 1 | Valid (minimum) |
| 100 | Valid (maximum) |
| 101 | Throw exception (too long) |

```java
@Test
public void descriptionTooShort() {
    assertThrows(IllegalArgumentException.class,
        () -> new Task(""));
}
```

---

## 9. Parameterized Testing

### The Problem Without Parameterized Tests

If you want to test the same behavior with multiple inputs, you'd normally write repetitive tests:

```java
@Test public void validA()     { assertNotNull(new Task("A")); }
@Test public void validTask()  { assertNotNull(new Task("Task")); }
@Test public void validStudy() { assertNotNull(new Task("Study")); }
```

That's a lot of duplicate code for essentially the same test.

### The Solution: `@ParameterizedTest`

JUnit 5 lets you run one test method with multiple inputs using `@ParameterizedTest`.

### With `@ValueSource` (simple values)

```java
@ParameterizedTest
@ValueSource(strings = {"A", "Task", "Study"})
public void validDescriptions(String input) {
    Task task = new Task(input);
    assertNotNull(task);
}
```

This runs three separate test cases — one for each value in the array.

For numbers:

```java
@ParameterizedTest
@ValueSource(ints = {0, 1, 10})
public void testValues(int value) {
    assertTrue(value >= 0);
}
```

### With `@MethodSource` (objects or complex inputs)

When you need to pass objects or multiple arguments, define a static method that returns a `Stream<Arguments>`:

```java
public static Stream<Arguments> taskCases() {
    return Stream.of(
        Arguments.of(new Task("Study"), true),
        Arguments.of(new Task("Sleep"), true)
    );
}

@ParameterizedTest
@MethodSource("taskCases")
public void testTaskCompletion(Task task, boolean expected) {
    task.complete();
    assertEquals(expected, task.isCompleted());
}
```

**Flow:** `@ParameterizedTest` → `Arguments` (the data) → `TestMethod` (runs once per argument set)

### Benefits of Parameterized Tests
- Reduces duplicate code
- Improves coverage with minimal effort
- Easy to add new test cases — just add to the data source

---

## 10. Testing Generic Classes

Generic classes use type parameters like `<T>`. At test time, you simply substitute a concrete type.

**The class being tested:**

```java
public class Box<T> {
    private T item;

    public void set(T item) { this.item = item; }
    public T get()          { return item; }
}
```

**Test with a `Task` type:**

```java
@Test
public void boxShouldStoreTask() {
    Box<Task> box = new Box<>();
    Task task = new Task("Study");

    box.set(task);

    assertEquals(task, box.get());
}
```

**Test with a `String` type:**

```java
@Test
public void boxShouldStoreString() {
    Box<String> box = new Box<>();
    box.set("Hello");
    assertEquals("Hello", box.get());
}
```

The generic class works for any type — your tests just pick a concrete type and verify behavior normally.

---

## 11. Test Setup and Teardown

When multiple tests in the same class share setup logic, you can extract it using lifecycle annotations instead of repeating yourself.

| Annotation | When it runs |
|------------|-------------|
| `@BeforeEach` | Before **every** test method |
| `@AfterEach` | After **every** test method |
| `@BeforeAll` | Once before **all** tests in the class (must be `static`) |
| `@AfterAll` | Once after **all** tests in the class (must be `static`) |

### Example

```java
private TaskList taskList;

@BeforeEach
public void setup() {
    taskList = new TaskList();   // fresh list for every test
}
```

This way, each test starts with a clean state.

---

## 12. Good Test Case Characteristics

A well-written test is:

| Property | Meaning |
|----------|---------|
| **Readable** | Anyone can understand what it's testing |
| **Independent** | Doesn't rely on other tests running first |
| **Deterministic** | Always produces the same result |
| **Small** | Tests exactly one behavior |

Use descriptive names:
- `shouldCompleteTask()` ✅
- `test1()` ❌

---

## 13. Mutation Testing

### The Limitation of Code Coverage

Traditional code coverage only tells you: *"did this line execute during testing?"*

It does **not** tell you whether your tests would actually catch a bug.

### What Is Mutation Testing?

Mutation testing deliberately introduces small bugs ("mutations") into your code and checks whether your tests fail. If they still pass after a mutation, your tests are **weak**.

**Example:**

```java
// Original
if (x > 10)

// Mutated
if (x >= 10)
```

If your tests still pass after this change, they didn't adequately check the boundary.

### Killed vs. Survived Mutations

| Result | Meaning |
|--------|---------|
| **Killed** | At least one test failed → your tests caught the bug ✅ |
| **Survived** | All tests still passed → your tests are weak ❌ |

### PIT (Pitest)

The tool used in this course is **PIT (Pitest)**. It:
- Automatically generates mutations
- Runs your tests against each mutant
- Reports which mutations were killed or survived
- Has a plugin for Eclipse

---

## 14. Regression Testing

**Regression testing** means re-running your existing test suite after making changes to the code, to ensure nothing that previously worked is now broken.

In real-world software, regression testing can take **weeks** and is very expensive — which is why having a good automated test suite matters so much.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Unit testing | Test one small piece of code in isolation |
| AAA pattern | Arrange → Act → Assert |
| Assertions | `assertEquals`, `assertTrue`, `assertThrows`, etc. |
| One behavior per test | Split complex tests into focused ones |
| Boundary value analysis | Test at and just outside the valid range |
| Parameterized tests | Run one test with many inputs using `@ParameterizedTest` |
| `@ValueSource` | Simple literal values (strings, ints) |
| `@MethodSource` | Objects or multi-argument test cases |
| Testing generics | Just substitute a concrete type when testing |
| `@BeforeEach` | Shared setup before every test |
| Mutation testing | Verify tests actually catch bugs, not just execute code |
| PIT | The mutation testing tool used in this course |