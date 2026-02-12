# Quiz 2: Week 8 & Week 4 (Inheritance up to "Is A" Test)

---

## Week 8: Object Relationships and Collaboration

### Question 1

A `Student` enrolls in `Courses`. Neither the student nor the course owns the other, and both can exist independently. What type of relationship is this?

- A) Composition
- B) Aggregation
- C) Association
- D) Inheritance

---

### Question 2

A `Car` has an `Engine`. The engine is created when the car is built and is destroyed when the car is scrapped. What type of relationship is this?

- A) Association
- B) Aggregation
- C) Composition
- D) Dependency

---

### Question 3

In UML, what does a **hollow (empty) diamond** represent?

- A) Composition
- B) Association
- C) Inheritance
- D) Aggregation

---

### Question 4

Which of the following questions helps you decide between aggregation and composition?

- A) How many methods does the class have?
- B) Can the parts exist independently of the whole?
- C) Is the class abstract?
- D) Does the class have a constructor?

---

### Question 5

The Law of Demeter is named after:

- A) A Greek philosopher who studied logic
- B) A research project at Northeastern University about adaptive programming
- C) The first programmer to use method chaining
- D) A design pattern from the Gang of Four book

---

### Question 6

Which of the following **follows** the Law of Demeter?

- A) `employee.getDepartment().getManager().getEmail();`
- B) `user.getProfile().getSettings().getTheme();`
- C) `customer.getEmailAddress();`
- D) `order.getCart().getItems().get(0).getPrice();`

---

### Question 7

What does **cohesion** measure?

- A) How many classes depend on each other
- B) How focused and purposeful a single class is
- C) The number of relationships between classes
- D) How many interfaces a class implements

---

### Question 8

Which of the following is a symptom of a **God Object**?

- A) The class has one clear responsibility
- B) The class has hundreds of methods and knows about many other classes
- C) The class follows the Law of Demeter
- D) The class has high cohesion

---

### Question 9

What does the phrase "only talk to your immediate friends, not strangers" refer to?

- A) The Information Expert principle
- B) The Single Responsibility Principle
- C) The Law of Demeter
- D) The Open/Closed Principle

---

### Question 10

A `TaskManager` needs to mark a task as complete. Which approach best follows good collaboration principles?

- A) `taskManager.getTaskList().getTasks().get(0).completed = true;`
- B) `task.setCompleted(true); task.setCompletedDate(new Date());`
- C) `task.markCompleted();`
- D) `taskList.setTaskCompleted(task, true, new Date());`

---

### Question 11

What is the term for a design where domain objects (like `Task`) hold data but contain no meaningful behavior?

- A) Rich domain model
- B) Anemic domain model
- C) Polymorphic model
- D) Abstract model

---

### Question 12

The fun rule of thumb says: "If your code reads like a sentence with many dots, it might be ___."

- A) Well-encapsulated
- B) Highly cohesive
- C) Gossiping too much
- D) Following best practices

---

---

## Week 4: Inheritance (up to "Is A" Test)

### Question 13

In Java, which of the following does a child class **inherit** from its parent?

- A) Only private fields
- B) All public and protected fields and methods
- C) Only static methods
- D) Only the constructor

---

### Question 14

Apply the **"Is A" test**. Which of the following is a **valid** use of inheritance?

- A) `Stack extends ArrayList`
- B) `Nurse extends Hospital`
- C) `Square extends Shape`
- D) `Keyboard extends Computer`

---

### Question 15

What are the three things inheritance gives you?

- A) Encapsulation, abstraction, and polymorphism
- B) Field/method reuse, method overriding, and subtype polymorphism
- C) Coupling, cohesion, and composition
- D) Constructors, destructors, and finalizers

---

---

## Answer Key

| Question | Answer | Explanation |
|----------|--------|-------------|
| 1  | **C** | Association — neither owns the other and both exist independently with no whole-part relationship. |
| 2  | **C** | Composition — the engine's lifecycle is tied to the car; it cannot exist independently. |
| 3  | **D** | A hollow diamond in UML represents aggregation (whole-part, parts independent). |
| 4  | **B** | The key distinction is whether parts can exist independently (aggregation) or not (composition). |
| 5  | **B** | The Law of Demeter is named after the Demeter Project at Northeastern University in the late 1980s. |
| 6  | **C** | `customer.getEmailAddress()` asks the immediate object directly without chaining through internals. |
| 7  | **B** | Cohesion measures how focused and purposeful a single class is. |
| 8  | **B** | A God Object has too many methods, knows about many classes, and acts as a central point of control. |
| 9  | **C** | "Only talk to your immediate friends, not strangers" is a metaphor for the Law of Demeter. |
| 10 | **C** | `task.markCompleted()` delegates behavior to the object that owns the data — good encapsulation. |
| 11 | **B** | An anemic domain model is when objects hold data but lack meaningful behavior. |
| 12 | **C** | Many dots (method chains) suggest the code is "gossiping" — reaching through too many objects. |
| 13 | **B** | A child class inherits all public and protected fields and methods from its parent. |
| 14 | **C** | A Square IS A Shape — this passes the "is a" test. The others fail it. |
| 15 | **B** | Inheritance provides field/method reuse, method overriding, and subtype polymorphism. |