# Practice Exam 1
## CMP_SC 3330 – Unit Testing & SOLID Principles (SRP, OCP, LSP)

**Instructions:** Choose the best answer for each question. Check your answers at the bottom.

---

### Question 1
What is the primary purpose of unit testing?

- A) To test how the entire application behaves end-to-end
- B) To test a small, isolated piece of code such as a method or class
- C) To verify that the database schema is correct
- D) To replace the need for code reviews

---

### Question 2
Which annotation marks a method as a JUnit 5 test?

- A) `@Run`
- B) `@TestCase`
- C) `@Test`
- D) `@JUnit`

---

### Question 3
In the AAA pattern, what does the **Act** phase represent?

- A) Creating objects and setting up test data
- B) Verifying the result with assertions
- C) Executing the single operation being tested
- D) Tearing down the test environment after execution

---

### Question 4
Which of the following is an example of a **bad** test, according to the lecture?

- A) A test with one `@BeforeEach` setup method
- B) A test that uses `@ParameterizedTest` with multiple inputs
- C) A test that adds two tasks and removes one, with two separate `assertEquals` calls
- D) A test named `shouldRejectNullTask()` that calls `assertThrows`

---

### Question 5
Your method accepts a task description between 1 and 100 characters. Which set of boundary values should you test?

- A) 1, 50, 100
- B) 0, 1, 100, 101
- C) -1, 0, 50, 100
- D) 1, 100 only

---

### Question 6
What is the main benefit of `@ParameterizedTest` over writing separate test methods for each input?

- A) It lets you skip the Assert phase
- B) It removes the need for the `@Test` annotation
- C) It reduces duplicate code and makes tests easier to extend
- D) It automatically generates boundary values for you

---

### Question 7
Which annotation would you use to supply simple string values to a parameterized test?

- A) `@MethodSource`
- B) `@CsvSource`
- C) `@ValueSource`
- D) `@EnumSource`

---

### Question 8
You want to pass `Task` objects as arguments to a parameterized test. Which approach should you use?

- A) `@ValueSource(objects = {...})`
- B) A static method returning `Stream<Arguments>` paired with `@MethodSource`
- C) `@ValueSource(classes = {Task.class})`
- D) Annotate the test with `@ObjectSource`

---

### Question 9
What does mutation testing actually measure?

- A) How many lines of code were executed during testing
- B) How many classes exist in the project
- C) Whether your tests would detect small bugs introduced into the code
- D) Whether your test method names follow naming conventions

---

### Question 10
In PIT mutation testing, a mutation is described as **"survived"**. What does this mean?

- A) At least one test failed after the mutation — the tests are strong
- B) All tests still passed after the mutation — the tests are weak
- C) The mutated code did not compile
- D) PIT skipped that line because it was already covered

---

### Question 11
Which lifecycle annotation runs **once** before all test methods in a class?

- A) `@BeforeEach`
- B) `@BeforeAll`
- C) `@SetupOnce`
- D) `@TestSetup`

---

### Question 12
You have a generic class `Box<T>`. How do you test it with JUnit 5?

- A) You cannot test generic classes with JUnit 5
- B) You must use reflection to bypass type erasure in the test
- C) You instantiate the class with a concrete type (e.g., `Box<String>`) and test normally
- D) You annotate the test class with `@GenericTest`

---

### Question 13
According to the Single Responsibility Principle, what is an **"axis of change"**?

- A) A method that returns a different value each time it's called
- B) A distinct responsibility that could cause a class to change independently
- C) A JUnit assertion that fails intermittently
- D) A design pattern used to separate UI from logic

---

### Question 14
A `UserManager` class handles user authentication, sends welcome emails, and writes audit logs. Which SOLID principle does this violate?

- A) Open/Closed Principle
- B) Liskov Substitution Principle
- C) Single Responsibility Principle
- D) Interface Segregation Principle

---

### Question 15
Which statement best describes the Open/Closed Principle?

- A) Classes should be open to modification whenever new features are needed
- B) Interfaces should be open for implementation by any class
- C) Software entities should be open for extension but closed for modification
- D) All methods should be public so external code can extend them

---

### Question 16
How does TDD support the Open/Closed Principle?

- A) TDD requires modifying existing tests every time requirements change
- B) TDD encourages adding new tests for new behavior while leaving existing passing tests untouched
- C) TDD eliminates the need for interfaces and abstract classes
- D) TDD closes classes from being inherited

---

### Question 17
A new payment type `CryptoPayment` is added to a system. According to OCP, how should this be handled?

- A) Edit the existing `PaymentProcessor` class to add a new `if` block for crypto
- B) Create a new class `CryptoPayment` that extends or implements the existing payment abstraction
- C) Delete the old payment class and rewrite it to support all types
- D) Add a `switch` statement inside the existing class

---

### Question 18
Which of the following best describes the Liskov Substitution Principle?

- A) A subclass should override every method in its parent class
- B) Any instance of a subclass should be usable wherever a parent class instance is expected, without breaking the program
- C) Parent classes should depend on their subclasses for implementation
- D) Interfaces should be substituted for abstract classes wherever possible

---

### Question 19
A `Rectangle` class has a method `setWidth(int w)` and `setHeight(int h)`. A `Square` subclass overrides both to always set width and height to the same value. This is a classic violation of which principle?

- A) Single Responsibility Principle
- B) Interface Segregation Principle
- C) Open/Closed Principle
- D) Liskov Substitution Principle

---

### Question 20
According to the lecture, using `instanceof` checks at runtime to handle different subtypes differently is a sign of what?

- A) Good polymorphic design
- B) A possible Liskov Substitution Principle violation
- C) Proper use of the Open/Closed Principle
- D) Correct application of boundary value analysis

---

## Answers

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | **B** | Unit testing focuses on a small, isolated piece of code — a method, class, or small component. |
| 2 | **C** | `@Test` is the JUnit 5 annotation that marks a method as a test case. |
| 3 | **C** | Act is the single operation being tested. Arrange sets up; Assert verifies. |
| 4 | **C** | Multiple actions (add, add, remove) and multiple assertions make the test purpose unclear. |
| 5 | **B** | Boundary value analysis tests the minimum (1), maximum (100), and just outside each (0, 101). |
| 6 | **C** | Parameterized tests reduce duplication and make it easy to add more cases. |
| 7 | **C** | `@ValueSource` supplies simple literal values like strings and ints. |
| 8 | **B** | For objects or multiple arguments, use a static `Stream<Arguments>` method with `@MethodSource`. |
| 9 | **C** | Mutation testing checks whether your tests would actually catch bugs — not just whether code was executed. |
| 10 | **B** | "Survived" means all tests still passed after the mutation — the tests failed to detect the bug. |
| 11 | **B** | `@BeforeAll` runs once before any test in the class. `@BeforeEach` runs before every individual test. |
| 12 | **C** | Simply instantiate the generic class with a concrete type (e.g., `Box<String>`) and test normally. |
| 13 | **B** | An axis of change is a distinct responsibility; if it changes, the class must change. |
| 14 | **C** | The class has three distinct responsibilities — a clear SRP violation. |
| 15 | **C** | OCP: open for extension (new behavior can be added) but closed for modification (existing code stays unchanged). |
| 16 | **B** | TDD adds new tests for new behavior without modifying existing passing tests — mirroring OCP. |
| 17 | **B** | OCP says extend behavior by creating new classes, not by editing existing working code. |
| 18 | **B** | LSP: a subtype must be substitutable for its parent without altering program correctness. |
| 19 | **D** | The `Square` changes the expected behavior of `setWidth`/`setHeight` — breaking LSP. |
| 20 | **B** | Runtime `instanceof` checks are a signal that subclasses aren't truly substitutable — an LSP violation. |