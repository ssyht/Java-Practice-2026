# CMP_SC 3330 – Midterm Exam Practice
**Covers: L1–L7 | Dr. Ekincan Ufuktepe | Mizzou**

> Mix of multiple choice, true/false, code prediction, and short answer.  
> Answers with full explanations are at the bottom.

---

## Part 1 – Java & OOP Foundations (L1)

**Q1.** Which of the following best describes the relationship between a *class* and an *object*?

- A) A class is an instance of an object
- B) An object is an instance of a class
- C) They are the same thing
- D) An object is a method inside a class

---

**Q2.** What does `static` mean when applied to a field or method?

- A) It can only be called from a constructor
- B) It belongs to the class itself, not to individual instances
- C) It cannot be changed after initialization
- D) It is accessible from every other package

---

**Q3 (True/False).** A `static` method can directly access an instance variable of the same class without a reference.

---

**Q4 (Code Prediction).** What is printed?

```java
public class Counter {
    static int count = 0;
    int id;

    Counter() {
        count++;
        id = count;
    }

    public static void main(String[] args) {
        Counter a = new Counter();
        Counter b = new Counter();
        Counter c = new Counter();
        System.out.println(Counter.count);
        System.out.println(b.id);
    }
}
```

---

## Part 2 – Classes, Encapsulation & Immutability (L2)

**Q5.** Which access modifier makes a field accessible only within the class it is declared in?

- A) `public`
- B) `protected`
- C) `private`
- D) `default` (no modifier)

---

**Q6.** What is the primary purpose of encapsulation?

- A) Make code run faster
- B) Hide internal state and only expose controlled access
- C) Allow a class to extend another class
- D) Eliminate the need for constructors

---

**Q7 (True/False).** Declaring all fields `final` is sufficient on its own to make a class immutable.

---

**Q8.** Consider this class:

```java
public class Money {
    private final int cents;

    public Money(int cents) {
        if (cents < 0) throw new IllegalArgumentException("Negative!");
        this.cents = cents;
    }

    public int getCents() { return cents; }
}
```

Which statement is true?

- A) `Money` is mutable because it has a getter
- B) `Money` is immutable — state cannot change after construction
- C) `Money` is invalid because `final` fields cannot have getters
- D) The `IllegalArgumentException` makes the class mutable

---

**Q9 (Short Answer).** List **three** benefits of immutable objects.

---

**Q10 (Code Prediction).** What happens when this code runs?

```java
public class Task {
    private String description;

    public Task(String description) {
        if (description == null || description.isBlank())
            throw new IllegalArgumentException("Bad description");
        this.description = description;
    }
}

// In main:
Task t = new Task("");
System.out.println("Created");
```

- A) Prints "Created"
- B) Prints nothing; the program silently exits
- C) Throws `IllegalArgumentException` before printing
- D) Compile error

---

## Part 3 – Object Relationships & Collaboration (L3)

**Q11.** What is the difference between **composition** and **aggregation**?

- A) They are synonyms — both mean "has-a"
- B) Composition means the child cannot exist without the parent; aggregation allows independent lifetimes
- C) Aggregation means the child cannot exist without the parent
- D) Composition applies only to interfaces

---

**Q12 (True/False).** In a UML class diagram, a filled (solid) diamond on a relationship line indicates aggregation.

---

**Q13.** A `Library` holds a list of `Book` objects. When the `Library` is destroyed, the `Book` objects still exist (they were published externally). This is:

- A) Composition
- B) Inheritance
- C) Aggregation
- D) Dependency

---

**Q14 (Code Trace).** Given:

```java
public class Engine {
    String type;
    Engine(String type) { this.type = type; }
}

public class Car {
    private Engine engine;
    Car(Engine e) { this.engine = e; }
    String getEngineType() { return engine.type; }
}

// In main:
Engine e = new Engine("V8");
Car c = new Car(e);
e.type = "V6";
System.out.println(c.getEngineType());
```

What is printed, and what design problem does this illustrate?

---

## Part 4 – Inheritance, Interfaces & Polymorphism (L4)

**Q15.** Which keyword is used to call a parent class constructor from a subclass constructor?

- A) `this()`
- B) `parent()`
- C) `super()`
- D) `base()`

---

**Q16.** What is the difference between **method overloading** and **method overriding**?

- A) Overloading is compile-time; overriding is runtime
- B) Overriding is compile-time; overloading is runtime
- C) They are the same
- D) Overloading applies only to constructors

---

**Q17 (True/False).** A class can `implement` multiple interfaces in Java.

---

**Q18 (True/False).** An abstract class can contain non-abstract (concrete) methods.

---

**Q19 (Code Prediction).** What is printed?

```java
class Animal {
    String speak() { return "..."; }
}

class Dog extends Animal {
    @Override
    String speak() { return "Woof"; }
}

class Cat extends Animal {
    @Override
    String speak() { return "Meow"; }
}

// In main:
Animal[] animals = { new Dog(), new Cat(), new Animal() };
for (Animal a : animals) {
    System.out.println(a.speak());
}
```

---

**Q20.** When you write `Animal a = new Dog();`, what is the **declared type** and what is the **runtime type**?

---

**Q21 (True/False).** `List<Dog>` is a subtype of `List<Animal>` in Java.

---

## Part 5 – Collections & Generics (L5 & L6)

**Q22.** Which `ArrayList` method removes an element at a specific index?

- A) `delete(int index)`
- B) `remove(int index)`
- C) `pop(int index)`
- D) `discard(int index)`

---

**Q23.** What does `HashMap.put(key, value)` return if the key already exists?

- A) `null`
- B) The new value just inserted
- C) The previous value that was associated with the key
- D) `false`

---

**Q24 (True/False).** A `HashSet` allows duplicate elements.

---

**Q25.** Why does `equals()` and `hashCode()` need to be consistent?

- A) Because the compiler requires it
- B) Because hash-based collections (HashMap, HashSet) use `hashCode()` to find the bucket and `equals()` to confirm identity — inconsistency causes elements to be "lost"
- C) Because `ArrayList` requires it for `sort()`
- D) They don't need to be consistent

---

**Q26.** What is **type erasure**?

- A) Java removes all type information from generic classes at compile time; at runtime, generics become raw types
- B) A way to delete unused variables
- C) The process of casting `Object` to a specific type
- D) A feature introduced in Java 21

---

**Q27.** What does **PECS** stand for, and when do you use each wildcard?

---

**Q28 (Code Prediction).** Does this compile? If not, why?

```java
List<Integer> ints = new ArrayList<>();
List<Number> nums = ints;   // ← this line
```

---

**Q29.** What is the difference between `<? extends T>` and `<? super T>`?

- A) `extends` = upper bound (read from); `super` = lower bound (write to)
- B) `super` = upper bound; `extends` = lower bound
- C) They are interchangeable
- D) `extends` allows writing; `super` allows reading

---

**Q30 (Code Prediction).** What is printed?

```java
public class Box<T> {
    private T item;
    public void set(T item) { this.item = item; }
    public T get() { return item; }
}

// In main:
Box<String> box = new Box<>();
box.set("Hello");
System.out.println(box.get().length());
```

---

## Part 6 – Unit Testing & JUnit 5 (L7)

**Q31.** Which annotation marks a method as a JUnit 5 test?

- A) `@TestCase`
- B) `@Testable`
- C) `@Test`
- D) `@Run`

---

**Q32.** What does `assertThrows(IllegalArgumentException.class, () -> new Task(""))` verify?

- A) That a `Task` can be constructed with an empty string
- B) That creating a `Task` with `""` throws `IllegalArgumentException`
- C) That `Task("")` returns null
- D) That `IllegalArgumentException` is in the classpath

---

**Q33.** Place the three steps of the **AAA pattern** in the correct order:

Options: Assert / Arrange / Act

---

**Q34 (True/False).** A well-designed unit test should test as many behaviors as possible in a single method to reduce code duplication.

---

**Q35.** Which annotation runs a setup method **before every individual test**?

- A) `@BeforeAll`
- B) `@SetUp`
- C) `@BeforeEach`
- D) `@Init`

---

**Q36.** For a field that accepts values 1–50, which four values would **boundary value analysis** tell you to test?

---

**Q37.** What is the difference between `@ValueSource` and `@MethodSource` in parameterized tests?

---

**Q38 (True/False).** Mutation testing asks "did the code run?" rather than "would the tests catch a bug?"

---

**Q39 (Code Prediction).** How many times does the test body execute?

```java
@ParameterizedTest
@ValueSource(strings = {"", " ", "abc", "hello", "x"})
void testInput(String s) {
    assertNotNull(s);
}
```

---

**Q40 (Short Answer).** Describe what a **mutation** is in the context of PIT mutation testing, and give one example of a mutation a tool might apply.

---

## Part 7 – Mixed / Tricky Questions

**Q41 (Code Prediction).** What is printed?

```java
class Vehicle {
    Vehicle() { System.out.println("Vehicle created"); }
}

class Truck extends Vehicle {
    Truck() {
        super();
        System.out.println("Truck created");
    }
}

// In main:
Truck t = new Truck();
```

---

**Q42.** What is wrong with this test?

```java
@Test
public void testTask() {
    Task task = new Task("Study");
    task.markCompleted();
    assertTrue(task.isCompleted());
    assertEquals("Study", task.getDescription());
    Task task2 = new Task("Read");
    assertFalse(task2.isCompleted());
    task2.markCompleted();
    assertTrue(task2.isCompleted());
}
```

---

**Q43 (True/False).** `@BeforeAll` methods must be declared `static`.

---

**Q44.** You have `List<? extends Number> list`. Can you call `list.add(42)`? Why or why not?

---

**Q45.** What is **polymorphism**, and what are the two types seen in Java?

---

---

# Answer Key

---

### Q1 → **B**
A class is the blueprint/template. An object (instance) is a concrete realization of that blueprint created with `new`.

---

### Q2 → **B**
`static` members belong to the class, not to any particular instance. All instances share the same static field.

---

### Q3 → **False**
A `static` method has no implicit `this` reference. It cannot directly access instance variables — it would need an object reference to do so.

---

### Q4 → Prints:
```
3
2
```
`count` is static and shared: after 3 constructors, it equals 3. `b` was the second object created, so `b.id = 2`.

---

### Q5 → **C**
`private` restricts access to the declaring class only.

---

### Q6 → **B**
Encapsulation hides internal state (`private` fields) and exposes controlled access through methods, preventing unintended modifications and reducing coupling.

---

### Q7 → **False**
`final` makes the *reference* immutable, not the referenced object. If a field is `final List<String> items`, the list itself can still be modified. True immutability also requires: no setters, defensive copies on input/output, making the class `final`.

---

### Q8 → **B**
`Money` is immutable. `cents` is `final`, the constructor validates and sets it once, and the getter only reads it. There is no way to change `cents` after construction.

---

### Q9 → Any three of:
1. Thread-safe by default (no synchronization needed)
2. Easier to test (predictable, no hidden state changes)
3. No hidden side effects (methods can't unexpectedly mutate shared state)
4. Safe to share across multiple references
5. Easier to reason about and debug

---

### Q10 → **C**
`description.isBlank()` returns `true` for `""`, so the `IllegalArgumentException` is thrown inside the constructor before `"Created"` can ever be printed.

---

### Q11 → **B**
Composition: the contained object's lifetime is tied to the container (e.g., a `Heart` inside a `Body`). Aggregation: the contained object can exist independently (e.g., `Book` inside `Library`).

---

### Q12 → **False**
A **filled (solid) diamond** indicates **composition**. An **open (hollow) diamond** indicates aggregation.

---

### Q13 → **C**
Because the `Book` objects outlive the `Library`, this is **aggregation** — the books have an independent lifecycle.

---

### Q14 → Prints: `V6`
**Design problem:** The `Car` holds a reference to the same `Engine` object as the caller. When `e.type` is mutated externally, the `Car`'s engine is also changed — this is a **reference aliasing / lack of defensive copy** problem. If `type` had been `private` or `Engine` was immutable, this couldn't happen.

---

### Q15 → **C**
`super()` calls the parent class constructor. It must be the first statement in the subclass constructor.

---

### Q16 → **A**
**Overloading** (same method name, different parameter list) is resolved at compile time (static dispatch). **Overriding** (subclass redefines a parent method) is resolved at runtime (dynamic dispatch).

---

### Q17 → **True**
Java allows a class to implement multiple interfaces: `class Foo implements A, B, C { }`.

---

### Q18 → **True**
Abstract classes can have both abstract methods (no body, must be overridden) and concrete methods (with a body, optionally overridden).

---

### Q19 → Prints:
```
Woof
Meow
...
```
Java resolves `speak()` at **runtime** based on the actual object type (polymorphism / dynamic dispatch), not the declared type `Animal`.

---

### Q20
- **Declared type:** `Animal` (the compile-time type used for method lookup)
- **Runtime type:** `Dog` (the actual object in memory, used for method dispatch)

---

### Q21 → **False**
Generics are **invariant** in Java. `List<Dog>` is NOT a subtype of `List<Animal>`. If it were, you could add a `Cat` into a `List<Dog>` through the `List<Animal>` reference, breaking type safety.

---

### Q22 → **B**
`list.remove(int index)` removes the element at the given position and shifts subsequent elements left.

---

### Q23 → **C**
`HashMap.put(key, value)` returns the **previous value** mapped to that key (or `null` if there was no previous mapping).

---

### Q24 → **False**
`HashSet` is built on the contract that all elements are unique. Adding a duplicate (as determined by `equals`/`hashCode`) is silently ignored.

---

### Q25 → **B**
Hash-based collections bucket objects by `hashCode()` first, then use `equals()` to check identity within the bucket. If two equal objects have different `hashCode`s, the collection cannot find one when looking up the other — the element appears "lost."

---

### Q26 → **A**
**Type erasure** is the process by which the Java compiler removes generic type parameters. At runtime, `List<String>` and `List<Integer>` are both just `List`. This maintains backward compatibility with pre-generics code but means you cannot do `new T()` or `instanceof List<String>` at runtime.

---

### Q27
**PECS = Producer Extends, Consumer Super**
- `<? extends T>` — use when the collection *produces* (you read from it). You cannot add to it (except `null`).
- `<? super T>` — use when the collection *consumes* (you write into it). Reading gives you `Object`.

---

### Q28 → **Does NOT compile**
```
List<Integer> ints = new ArrayList<>();
List<Number> nums = ints;   // ← compile error
```
Generics are invariant. `List<Integer>` is not a subtype of `List<Number>`. Use `List<? extends Number>` if you want to accept both.

---

### Q29 → **A**
- `<? extends T>` (upper bound): the wildcard represents T or any subtype. Safe to **read** as T, **unsafe to write** (you don't know the exact subtype).
- `<? super T>` (lower bound): the wildcard represents T or any supertype. Safe to **write** T values in, **unsafe to read** as anything more specific than Object.

---

### Q30 → Prints: `5`
`box.get()` returns `"Hello"` (a `String`), and `"Hello".length()` is `5`.

---

### Q31 → **C**
`@Test` (from `org.junit.jupiter.api.Test`) marks a method as a JUnit 5 test case.

---

### Q32 → **B**
`assertThrows(ExceptionType.class, lambda)` passes only if executing the lambda throws an exception of exactly that type. It fails if no exception is thrown or a different exception is thrown.

---

### Q33 → **Arrange → Act → Assert**
1. **Arrange** – set up objects, data, initial state
2. **Act** – call the one method under test
3. **Assert** – verify the result

---

### Q34 → **False**
A well-designed test should test **one behavior**. Tests with multiple unrelated assertions are harder to understand and harder to diagnose when they fail.

---

### Q35 → **C**
`@BeforeEach` runs the annotated method before every single test method in the class, ensuring each test starts with a fresh state.

---

### Q36 → Test: `0, 1, 50, 51`
- `0` — just below minimum (should reject)
- `1` — minimum valid (should accept)
- `50` — maximum valid (should accept)
- `51` — just above maximum (should reject)

---

### Q37
- `@ValueSource` — inline primitive or string values directly in the annotation. Good for simple, flat lists of values.
- `@MethodSource("methodName")` — points to a static factory method returning `Stream<Arguments>`. Needed for objects, multiple parameters, or complex data.

---

### Q38 → **False**
Traditional code coverage asks "did the code run?" Mutation testing asks "would the tests *catch a bug*?" — it's a stronger measure of test quality.

---

### Q39 → **5 times**
One execution per value in `@ValueSource`. There are 5 strings, so the test body runs 5 times.

---

### Q40
A **mutation** is a small, automatic modification to the source code designed to simulate a real bug — e.g., changing `>` to `>=`, negating a boolean return, removing a line.

**Example:** Original: `if (length > 100)` → Mutant: `if (length >= 100)`

If your tests still pass after this mutation, they are not checking the boundary precisely enough. PIT reports a **mutation score** = killed mutants / total mutants.

---

### Q41 → Prints:
```
Vehicle created
Truck created
```
When `new Truck()` is called, `super()` in `Truck`'s constructor invokes `Vehicle()`'s constructor first, which prints "Vehicle created". Then "Truck created" is printed.

---

### Q42
The test violates the **one behavior per test** principle. It tests two separate tasks (`task` and `task2`) and multiple distinct behaviors (marking complete, reading description, default completion state) all in one method. Split into at least 3 separate tests:
- `shouldMarkTaskCompleted()`
- `shouldReturnCorrectDescription()`
- `newTaskShouldNotBeCompleted()`

---

### Q43 → **True**
`@BeforeAll` methods must be `static` because they run once for the whole class — before any instance is created.

---

### Q44 → **No, you cannot.**
With `List<? extends Number>`, the compiler doesn't know the exact type (it could be `List<Integer>`, `List<Double>`, etc.). Adding any value (except `null`) is forbidden because it might violate type safety. This wildcard is for **reading only** (producer extends).

---

### Q45
**Polymorphism** is the ability for a reference of one type to refer to objects of different types and invoke the correct behavior at runtime.

**Two types in Java:**
1. **Compile-time polymorphism (static)** — method overloading: same name, different parameter signatures, resolved at compile time.
2. **Runtime polymorphism (dynamic)** — method overriding: subclass redefines a parent method, and Java dispatches to the correct version at runtime based on the actual object type.