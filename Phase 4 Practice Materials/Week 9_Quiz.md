# Quiz: Week 8 & Week 4 (Inheritance up to "Is A" Test)

---

## Week 8: Object Relationships and Collaboration

### Question 1

What are the three ways objects solve problems in OOP?

- A) Storing data, printing output, reading input
- B) Knowing things, doing things, asking other objects for help
- C) Compiling, executing, terminating
- D) Creating classes, writing methods, declaring variables

---

### Question 2

Which type of relationship is described as: "One object knows about another, but there is no ownership implied"?

- A) Composition
- B) Aggregation
- C) Association
- D) Inheritance

---

### Question 3

A `Team` has `Players`. If the team is disbanded, the players continue to exist. What type of relationship is this?

- A) Association
- B) Composition
- C) Aggregation
- D) Dependency

---

### Question 4

A `House` has `Rooms`. If the house is demolished, the rooms cease to exist. What type of relationship is this?

- A) Association
- B) Aggregation
- C) Composition
- D) Inheritance

---

### Question 5

In UML, what does a **filled (solid) diamond** represent?

- A) Association
- B) Aggregation
- C) Inheritance
- D) Composition

---

### Question 6

What is the Law of Demeter also known as?

- A) "Talk to Everyone"
- B) "Principle of Least Knowledge"
- C) "Principle of Maximum Reuse"
- D) "The Open/Closed Principle"

---

### Question 7

Which of the following **violates** the Law of Demeter?

- A) `order.getTaxRate();`
- B) `order.getCustomer().getAddress().getZipCode().getTaxRate();`
- C) `task.markCompleted();`
- D) `taskList.addTask(newTask);`

---

### Question 8

According to the Law of Demeter, an object should **only** talk to:

- A) Any object it can access through method chaining
- B) Itself, its fields, its method parameters, and objects it creates
- C) Only objects of the same class
- D) Only objects that are passed as constructor arguments

---

### Question 9

What is **coupling**?

- A) How focused and purposeful a class is
- B) The number of methods in a class
- C) The degree of dependency between classes
- D) The number of fields in a class

---

### Question 10

In good OOP design, you should aim for:

- A) High coupling, high cohesion
- B) Low coupling, low cohesion
- C) High coupling, low cohesion
- D) Low coupling, high cohesion

---

### Question 11

What is wrong with the following code?

```java
if (task.getCompleted() == false) {
    task.setCompleted(true);
    task.setCompletedDate(new Date());
}
```

- A) The `if` condition has a syntax error
- B) The caller is manipulating Task's internal state instead of delegating behavior to Task
- C) `getCompleted()` should return a `String`, not a `boolean`
- D) Nothing is wrong; this is best practice

---

### Question 12

What is the **improved** version of the code from Question 11?

- A) `taskManager.completeTask(task);`
- B) `task.setCompleted(true);`
- C) `task.markCompleted();`
- D) `taskList.getTask().setCompleted(true);`

---

### Question 13

The **Information Expert** principle states that responsibilities should be assigned to:

- A) The class with the most methods
- B) The class that has the most information needed to fulfill the responsibility
- C) The manager or controller class
- D) The class that was created first

---

### Question 14

Which of the following is a **code smell** indicating poor collaboration?

- A) A class with a single responsibility
- B) Using `task.markCompleted()` to complete a task
- C) `user.getAccount().getProfile().getAddress().getZipCode();`
- D) Keeping relationships unidirectional

---

### Question 15

What is a **God Object**?

- A) An object that follows the Law of Demeter perfectly
- B) An abstract class with no implementations
- C) A class that knows too much and does too much
- D) An object that has no dependencies

---

### Question 16

Which design principle is summarized as "behavior lives with the data"?

- A) Objects that own the data should provide behavior to modify it
- B) All logic should be in the main method
- C) Data and behavior should always be in separate classes
- D) Manager classes should handle all behavior

---

### Question 17

Which is a common **relationship mistake** to avoid?

- A) Keeping relationships simple
- B) Preferring unidirectional relationships
- C) Adding bidirectional relationships everywhere
- D) Designing before you code

---

---

## Week 4: Inheritance (up to "Is A" Test)

### Question 18

What does inheritance allow a child class to do?

- A) Only access private fields of the parent class
- B) Acquire the properties and methods of a parent class
- C) Replace the parent class entirely
- D) Remove methods from the parent class

---

### Question 19

Which keyword is used in Java to establish an inheritance relationship?

- A) `implements`
- B) `inherits`
- C) `extends`
- D) `super`

---

### Question 20

What is the purpose of the `@Override` annotation?

- A) It creates a new method in the parent class
- B) It indicates you are intentionally overriding a parent method and helps the compiler detect errors
- C) It prevents a method from being inherited
- D) It makes a method private

---

### Question 21

What happens if you use `@Override` on a method that does **not** actually override a parent method?

- A) The code runs normally
- B) A runtime exception is thrown
- C) A compiler error is generated
- D) The method is automatically deleted

---

### Question 22

Which of the following is a valid reason to use inheritance?

- A) To avoid writing any new code
- B) To share common behavior and model "is a" relationships
- C) To make all fields public
- D) To combine unrelated classes

---

### Question 23

Apply the **"Is A" test**. Which of the following is a **valid** use of inheritance?

- A) `Car extends Wheel`
- B) `Dog extends Animal`
- C) `Laptop extends Battery`
- D) `Engine extends Car`

---

### Question 24

Apply the **"Is A" test**. Which of the following is **NOT** a valid use of inheritance?

- A) `Warrior extends GameCharacter`
- B) `Dog extends Animal`
- C) `Car extends Wheel`
- D) `Cat extends Animal`

---

### Question 25

Given the following code, what is the output?

```java
public class Animal {
    void speak() {
        System.out.println("...");
    }
}

public class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("Woof");
    }
}

// In main:
Animal a = new Dog();
a.speak();
```

- A) `...`
- B) `Woof`
- C) Compiler error
- D) Nothing is printed

---

---

## Answer Key

| Question | Answer | Explanation |
|----------|--------|-------------|
| 1  | **B** | Objects know things (state), do things (behavior), and ask other objects for help (collaboration). |
| 2  | **C** | Association means objects know about each other with no ownership implied. |
| 3  | **C** | Aggregation — parts (players) exist independently of the whole (team). |
| 4  | **C** | Composition — parts (rooms) cannot exist without the whole (house). |
| 5  | **D** | A filled diamond in UML represents composition (strong ownership). |
| 6  | **B** | The Law of Demeter is also called the "Principle of Least Knowledge." |
| 7  | **B** | Chaining through multiple objects violates the Law of Demeter. |
| 8  | **B** | An object should only talk to itself, its fields, its parameters, and objects it creates. |
| 9  | **C** | Coupling is the degree of dependency between classes. |
| 10 | **D** | Good design has low coupling (between classes) and high cohesion (within classes). |
| 11 | **B** | The caller manipulates Task's internals instead of letting Task handle its own state. |
| 12 | **C** | `task.markCompleted()` delegates the behavior to the Task object itself. |
| 13 | **B** | The Information Expert principle assigns responsibility to the class with the most relevant information. |
| 14 | **C** | Long method chains are a code smell indicating poor collaboration and Law of Demeter violations. |
| 15 | **C** | A God Object knows too much and does too much, violating single responsibility. |
| 16 | **A** | The object that owns the data should provide the behavior to modify it. |
| 17 | **C** | Bidirectional relationships everywhere add unnecessary complexity and circular dependencies. |
| 18 | **B** | Inheritance allows a child class to acquire properties and methods of a parent class. |
| 19 | **C** | The `extends` keyword establishes inheritance in Java. |
| 20 | **B** | `@Override` signals intentional overriding and helps the compiler catch errors. |
| 21 | **C** | A compiler error is generated if the method doesn't actually override a parent method. |
| 22 | **B** | Inheritance is used to share common behavior and model "is a" relationships. |
| 23 | **B** | A Dog IS AN Animal — this is a valid "is a" relationship. |
| 24 | **C** | A Car IS NOT A Wheel — this fails the "is a" test. |
| 25 | **B** | Polymorphism: the `Dog` version of `speak()` is called, printing "Woof". |