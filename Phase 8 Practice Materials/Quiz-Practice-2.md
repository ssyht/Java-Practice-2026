# Practice Exam 2
## CMP_SC 3330 – Unit Testing & SOLID Principles (SRP, OCP, LSP)

**Instructions:** Choose the best answer for each question. Check your answers at the bottom.

---

### Question 1
Which of the following is NOT a benefit of testing listed in the lecture?

- A) Catch bugs early
- B) Prevent regressions
- C) Eliminate the need for documentation
- D) Improve design

---

### Question 2
Where should test classes be placed in a Maven project?

- A) `src/main/java`
- B) `src/test/resources`
- C) `src/test/java`
- D) `src/main/test`

---

### Question 3
A test class is named `InvoiceService.java`. Following course naming conventions, what should its test class be named?

- A) `InvoiceTest.java`
- B) `TestInvoiceService.java`
- C) `InvoiceServiceTest.java`
- D) `InvoiceServiceSpec.java`

---

### Question 4
You want to assert that calling `withdraw(-50)` on a `BankAccount` throws an `IllegalArgumentException`. Which assertion is correct?

- A) `assertEquals(IllegalArgumentException.class, account.withdraw(-50))`
- B) `assertThrows(IllegalArgumentException.class, () -> account.withdraw(-50))`
- C) `assertTrue(account.withdraw(-50) throws IllegalArgumentException)`
- D) `assertException(account.withdraw(-50), IllegalArgumentException.class)`

---

### Question 5
In the AAA pattern, what belongs in the **Arrange** phase?

- A) Calling the method being tested
- B) Writing the assertion that checks the result
- C) Creating objects, initializing data, and defining input values
- D) Running the test runner

---

### Question 6
Which of the following test method names best follows the course's naming conventions?

- A) `test1()`
- B) `taskTest()`
- C) `shouldRejectInvalidTask()`
- D) `InvalidTaskRejection()`

---

### Question 7
A test for a method that accepts ages 0–120 should test which boundary values?

- A) 0 and 120 only
- B) -1, 0, 120, 121
- C) 1, 60, 119
- D) 0, 60, 120

---

### Question 8
Which of the following is a characteristic of a **good** test case?

- A) It tests as many behaviors as possible in one method
- B) It depends on the result of a previously run test
- C) It is small, readable, independent, and deterministic
- D) It uses `System.out.println` to display results instead of assertions

---

### Question 9
What does `@BeforeEach` do?

- A) Runs a method once after all tests in the class have completed
- B) Runs a method before every individual test method in the class
- C) Marks a method as a test that should be run first
- D) Sets up the test runner configuration

---

### Question 10
Consider this parameterized test:

```java
@ParameterizedTest
@ValueSource(strings = {"", " ", "ab"})
public void shouldRejectShortOrEmpty(String input) {
    assertThrows(IllegalArgumentException.class, () -> new Task(input));
}
```

How many individual test cases does JUnit run from this method?

- A) 1
- B) 2
- C) 3
- D) It depends on the `Task` implementation

---

### Question 11
What is the difference between **code coverage** and **mutation testing**?

- A) Code coverage checks for runtime exceptions; mutation testing checks for compilation errors
- B) Code coverage tells you if lines were executed; mutation testing tells you if your tests would actually catch bugs
- C) Code coverage is used only with generics; mutation testing applies to all classes
- D) They are the same thing — just different names for the same metric

---

### Question 12
A mutation changes `>=` to `>` in your production code and all your tests still pass. What should you conclude?

- A) Your tests are strong — they correctly ignored an irrelevant change
- B) Your tests are weak — they failed to detect a boundary bug
- C) The mutation was invalid and should be ignored
- D) PIT made an error; this type of change cannot be a mutation

---

### Question 13
Which of the following best describes **regression testing**?

- A) Testing a single class in isolation before it is integrated
- B) Re-running existing tests after code changes to ensure nothing previously working is now broken
- C) Running only the newest tests written for a new feature
- D) Testing edge cases at the boundaries of valid input

---

### Question 14
According to SRP, which refactoring is most appropriate for this class?

```java
public class Report {
    public void generateReport() { ... }
    public void saveToDatabase() { ... }
    public void sendByEmail() { ... }
}
```

- A) Make all methods `static`
- B) Add more methods to consolidate behavior further
- C) Split into separate classes: `ReportGenerator`, `ReportRepository`, `ReportMailer`
- D) Combine all methods into a single `run()` method

---

### Question 15
How does TDD support the Single Responsibility Principle?

- A) TDD forces you to write all tests in a single class
- B) If a class is hard to test in isolation, TDD reveals it has too many responsibilities and needs to be split
- C) TDD removes the need for classes to have any responsibility at all
- D) TDD restricts how many methods a class can have

---

### Question 16
A developer adds a new feature by editing a class that was already fully tested and working. Which principle does this approach risk violating?

- A) Liskov Substitution Principle
- B) Interface Segregation Principle
- C) Open/Closed Principle
- D) Single Responsibility Principle

---

### Question 17
Which of the following design tools does the Open/Closed Principle primarily rely on in Java?

- A) `static` methods and global variables
- B) Interfaces and abstract classes to allow extension without modification
- C) `instanceof` checks to handle each new type
- D) Overloaded constructors in the base class

---

### Question 18
A `Bird` base class has a method `fly()`. A `Penguin` subclass extends `Bird` but throws `UnsupportedOperationException` from `fly()`. Which principle is violated?

- A) Single Responsibility Principle
- B) Open/Closed Principle
- C) Liskov Substitution Principle
- D) Dependency Inversion Principle

---

### Question 19
According to the Liskov Substitution Principle, a subclass should:

- A) Override every method of the parent class, even if it changes the behavior completely
- B) Be usable wherever the parent type is expected, without breaking correct program behavior
- C) Always be declared `abstract` to prevent direct instantiation
- D) Depend directly on other subclasses rather than the parent class

---

### Question 20
You are writing tests for an interface `Shape` with a method `area()`. You write tests that pass for `Circle` and `Rectangle`. A new class `Triangle implements Shape` is added. According to LSP, what should be true?

- A) You need to rewrite all existing `Shape` tests for `Triangle`
- B) `Triangle` should pass the same behavioral expectations as `Circle` and `Rectangle` when used as a `Shape`
- C) `Triangle` is exempt from LSP because it has three sides
- D) You must use `instanceof Triangle` in the tests to handle it correctly

---

## Answers

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | **C** | The lecture lists: catch bugs early, prevent regressions, improve design, document behavior. Eliminating documentation is not listed. |
| 2 | **C** | Maven convention: production code in `src/main/java`, test code in `src/test/java`. |
| 3 | **C** | The course convention is to append `Test` to the production class name: `InvoiceServiceTest.java`. |
| 4 | **B** | `assertThrows` takes the expected exception class and a lambda containing the code that should throw. |
| 5 | **C** | Arrange is where you create objects, initialize data, and set up inputs — everything needed before the action. |
| 6 | **C** | Descriptive, behavior-focused names like `shouldRejectInvalidTask()` follow course conventions. |
| 7 | **B** | BVA for 0–120: test just below min (-1), at min (0), at max (120), and just above max (121). |
| 8 | **C** | Good tests are small, readable, independent (no reliance on other tests), and deterministic (same result every time). |
| 9 | **B** | `@BeforeEach` runs before every individual test method — useful for resetting shared state. |
| 10 | **C** | One test case is run per value in the `@ValueSource` array — so 3 values = 3 test runs. |
| 11 | **B** | Coverage = did the line run? Mutation testing = would the test fail if a bug were introduced? |
| 12 | **B** | If tests pass after a mutation, the mutation "survived" — the tests are weak and missed a boundary case. |
| 13 | **B** | Regression testing re-runs existing tests after changes to ensure previously working features still work. |
| 14 | **C** | SRP says each class should have one responsibility. Split into three focused classes, one per concern. |
| 15 | **B** | If a class is hard to isolate and test, TDD reveals it likely has multiple responsibilities — a SRP violation. |
| 16 | **C** | OCP says once code is working, extend it rather than modify it. Editing working code risks breaking it. |
| 17 | **B** | OCP is achieved in Java through interfaces and abstract classes — new behavior is added by extension, not by editing. |
| 18 | **C** | `Penguin` cannot be substituted for `Bird` wherever `fly()` is expected — a classic LSP violation. |
| 19 | **B** | LSP: subclasses must be drop-in replacements for their parent type without changing program correctness. |
| 20 | **B** | LSP means any `Shape` — including `Triangle` — must satisfy the behavioral contract defined by the `Shape` interface. |