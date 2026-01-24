# Phase 2: CS3330 Course Mastery (Weeks 5-16)

## Overview
This phase is designed to make you **excel** in your CS3330 course by covering every single topic in depth with practice exercises and real-world applications.

---

## Week 5: Java and OOP Foundations
**CS3330 Week 1 Coverage**

### Topics:
- Review of Java basics in OOP context
- What is Object-Oriented Programming?
- The four pillars: Encapsulation, Inheritance, Polymorphism, Abstraction
- Classes vs Objects
- Creating your first class
- Instance variables vs class variables
- Methods: instance methods vs static methods
- The `this` keyword

### Practice Projects:
- Create a `Person` class with properties and methods
- Build a `BankAccount` class with deposit/withdraw functionality
- Design a `Car` class with attributes and behaviors

---

## Week 6: Classes, Encapsulation, and Immutability
**CS3330 Week 2 Coverage**

### Topics:
- **Encapsulation:**
  - Private, public, protected access modifiers
  - Getters and setters
  - Why encapsulation matters
  - Information hiding
  
- **Constructors:**
  - Default constructors
  - Parameterized constructors
  - Constructor overloading
  - Copy constructors
  
- **Immutability:**
  - What are immutable objects?
  - The `final` keyword
  - Creating immutable classes
  - Benefits of immutability
  - String immutability

### Practice Projects:
- Create an immutable `Date` class
- Build a `Student` class with proper encapsulation
- Design a `Rectangle` class with getters/setters and validation

---

## Week 7: Object Relationships and Collaboration
**CS3330 Week 3 Coverage**

### Topics:
- **Types of Relationships:**
  - Association
  - Aggregation (has-a relationship)
  - Composition (part-of relationship)
  - Dependency
  
- **Object Collaboration:**
  - Objects working together
  - Method calls between objects
  - Passing objects as parameters
  - Returning objects from methods
  
- **UML Class Diagrams:**
  - Reading class diagrams
  - Relationships in UML
  - Creating basic diagrams

### Practice Projects:
- University system: `Student`, `Course`, `Professor` classes
- Library system: `Book`, `Member`, `Library` classes
- Car rental: `Car`, `Customer`, `Rental` classes

---

## Week 8: Inheritance, Interfaces, and Polymorphism
**CS3330 Week 4 Coverage**

### Topics:
- **Inheritance:**
  - `extends` keyword
  - Parent class (superclass) and child class (subclass)
  - Method overriding
  - `super` keyword
  - Constructor chaining
  - The `Object` class
  
- **Interfaces:**
  - What are interfaces?
  - `implements` keyword
  - Multiple interface implementation
  - Default methods in interfaces
  - When to use interface vs inheritance
  
- **Polymorphism:**
  - Compile-time polymorphism (method overloading)
  - Runtime polymorphism (method overriding)
  - Dynamic binding
  - Upcasting and downcasting
  - The `instanceof` operator

### Practice Projects:
- Shape hierarchy: `Shape`, `Circle`, `Rectangle`, `Triangle`
- Employee system: `Employee`, `Manager`, `Developer`, `Intern`
- Animal kingdom: `Animal`, `Dog`, `Cat`, `Bird` with interfaces

---

## Week 9: Collections Framework
**CS3330 Week 5 Coverage**

### Topics:
- **List Interface:**
  - ArrayList
  - LinkedList
  - When to use which
  - Common operations
  
- **Set Interface:**
  - HashSet
  - TreeSet
  - LinkedHashSet
  - No duplicates
  
- **Map Interface:**
  - HashMap
  - TreeMap
  - LinkedHashMap
  - Key-value pairs
  
- **Queue Interface:**
  - Queue basics
  - PriorityQueue
  - Deque
  
- **Iterating Collections:**
  - For-each loop
  - Iterator
  - ListIterator

### Practice Projects:
- Contact management system using HashMap
- To-do list using ArrayList
- Unique word counter using HashSet
- Student grade tracker using TreeMap

---

## Week 10: Introduction to Generics
**CS3330 Week 5 Coverage (continued)**

### Topics:
- Why generics?
- Type safety
- Generic classes
- Generic methods
- Generic interfaces
- Type parameters (T, E, K, V)
- Using generics with collections
- Benefits of generics

### Practice Projects:
- Create a generic `Box<T>` class
- Generic `Pair<K, V>` class
- Generic stack implementation
- Generic method for array reversal

---

## Week 11: Generics Deep Dive
**CS3330 Week 6 Coverage**

### Topics:
- **Bounded Type Parameters:**
  - Upper bounds (`extends`)
  - Lower bounds (`super`)
  - Multiple bounds
  
- **Wildcards:**
  - Unbounded wildcards (`?`)
  - Upper bounded wildcards (`? extends T`)
  - Lower bounded wildcards (`? super T`)
  
- **Generic Restrictions:**
  - Cannot instantiate generic types
  - Cannot create arrays of parameterized types
  - Cannot use primitives
  
- **Type Erasure:**
  - How Java implements generics
  - Runtime implications

### Practice Projects:
- Generic sorting algorithm
- Generic data structure (LinkedList<T>)
- Bounded type parameter exercises
- Wildcard practice problems

---

## Week 12: Unit Testing and Coverage
**CS3330 Week 7 Coverage**

### Topics:
- **Testing Fundamentals:**
  - Why test?
  - Types of testing
  - Test-Driven Development (TDD)
  
- **JUnit:**
  - Setting up JUnit
  - Writing test cases
  - Assertions (`assertEquals`, `assertTrue`, etc.)
  - `@Test`, `@Before`, `@After` annotations
  - Test fixtures
  
- **Code Coverage:**
  - What is code coverage?
  - Line coverage
  - Branch coverage
  - Coverage tools
  - Aiming for high coverage
  
- **Best Practices:**
  - AAA pattern (Arrange, Act, Assert)
  - Testing edge cases
  - Testing exceptions
  - Parameterized tests

### Practice Projects:
- Write tests for Calculator class
- Test BankAccount with edge cases
- Test String utilities
- Achieve 100% coverage on a class

---

## Week 13: SOLID Principles
**CS3330 Week 8 Coverage**

### Topics:
- **S - Single Responsibility Principle:**
  - One class, one reason to change
  - Separating concerns
  - Examples and violations
  
- **O - Open/Closed Principle:**
  - Open for extension, closed for modification
  - Using inheritance and interfaces
  
- **L - Liskov Substitution Principle:**
  - Substitutability of subclasses
  - Behavioral subtyping
  
- **I - Interface Segregation Principle:**
  - Many specific interfaces > one general interface
  - Client-specific interfaces
  
- **D - Dependency Inversion Principle:**
  - Depend on abstractions, not concretions
  - Dependency injection

### Practice Projects:
- Refactor code to follow SRP
- Design extensible system using OCP
- Create interface-segregated design
- Implement dependency injection

---

## Week 14: GRASP Principles
**CS3330 Week 8 Coverage (continued)**

### Topics:
- **Information Expert:**
  - Assign responsibility to the class with the information
  
- **Creator:**
  - Who creates objects?
  
- **Controller:**
  - First object beyond UI that handles system events
  
- **Low Coupling:**
  - Minimize dependencies between classes
  
- **High Cohesion:**
  - Keep related functionality together
  
- **Polymorphism:**
  - Use polymorphic behavior instead of conditionals
  
- **Pure Fabrication:**
  - Create classes not in the problem domain
  
- **Indirection:**
  - Use intermediary to decouple classes
  
- **Protected Variations:**
  - Shield from variations

### Practice Projects:
- Apply GRASP to e-commerce system
- Design point-of-sale system
- Refactor existing code with GRASP
- Create loosely coupled architecture

---

## Week 15: CRUD and Persistence
**CS3330 Week 9 Coverage**

### Topics:
- **CRUD Operations:**
  - Create, Read, Update, Delete
  
- **File I/O:**
  - Reading from files
  - Writing to files
  - BufferedReader and BufferedWriter
  - Try-with-resources
  
- **Serialization:**
  - Object serialization
  - Serializable interface
  - transient keyword
  - ObjectOutputStream/ObjectInputStream
  
- **Database Basics:**
  - Introduction to databases
  - JDBC basics
  - Connecting to database
  - Executing queries
  - PreparedStatement

### Practice Projects:
- Save and load student records to file
- Serialize objects to disk
- Simple database CRUD application
- Contact manager with file persistence

---

## Week 16: Design Patterns I
**CS3330 Week 11 Coverage**

### Topics:
- **What are Design Patterns?**
  - Gang of Four (GoF)
  - Creational, Structural, Behavioral patterns
  
- **Singleton Pattern:**
  - One instance per application
  - Thread-safe implementation
  
- **Factory Pattern:**
  - Object creation delegation
  - Factory method
  
- **Builder Pattern:**
  - Complex object construction
  - Fluent interfaces
  
- **Observer Pattern:**
  - Event handling
  - Subject-Observer relationship
  
- **Strategy Pattern:**
  - Encapsulating algorithms
  - Runtime algorithm selection

### Practice Projects:
- Implement Singleton logger
- Create factory for different shapes
- Build complex objects with Builder
- Event system with Observer
- Multiple sorting strategies

---

## Week 17: GUI Programming with Swing
**CS3330 Week 12 Coverage**

### Topics:
- **Swing Basics:**
  - JFrame, JPanel
  - Layout managers (FlowLayout, BorderLayout, GridLayout)
  - Components (JButton, JLabel, JTextField, JTextArea)
  
- **Event Handling:**
  - ActionListener
  - Event-driven programming
  - Anonymous inner classes
  - Lambda expressions for listeners
  
- **Advanced Components:**
  - JList, JComboBox
  - JTable
  - JMenuBar, JMenu
  - JDialog
  
- **Custom Painting:**
  - Overriding paintComponent
  - Graphics object
  - Drawing shapes

### Practice Projects:
- Simple calculator GUI
- To-do list application
- Drawing program
- Student management system GUI

---

## Week 18: MVC Architecture
**CS3330 Week 13 Coverage**

### Topics:
- **MVC Pattern:**
  - Model (data and business logic)
  - View (UI presentation)
  - Controller (handles input, updates model/view)
  
- **Separation of Concerns:**
  - Why separate?
  - Benefits of MVC
  
- **Implementing MVC:**
  - Model classes
  - View classes (Swing)
  - Controller classes
  - Communication between components
  
- **Observer Pattern in MVC:**
  - Model notifies View of changes
  - PropertyChangeListener

### Practice Projects:
- Calculator with MVC
- Student management system with MVC
- Shopping cart application with MVC
- Game with MVC architecture

---

## Week 19: Functional Programming in Java
**CS3330 Week 14 Coverage**

### Topics:
- **Lambda Expressions:**
  - Syntax
  - Functional interfaces
  - Method references
  
- **Streams API:**
  - Stream creation
  - Intermediate operations (filter, map, sorted)
  - Terminal operations (collect, forEach, reduce)
  - Parallel streams
  
- **Optional Class:**
  - Handling null safely
  - Optional methods
  
- **Functional Interfaces:**
  - Predicate, Function, Consumer, Supplier
  - Custom functional interfaces

### Practice Projects:
- Filter and transform collections with streams
- Process large datasets
- Replace loops with streams
- Create functional-style APIs

---

## Week 20: Design Patterns II
**CS3330 Week 15 Coverage**

### Topics:
- **Decorator Pattern:**
  - Adding functionality dynamically
  
- **Adapter Pattern:**
  - Making incompatible interfaces work
  
- **Template Method Pattern:**
  - Algorithm skeleton in base class
  
- **Command Pattern:**
  - Encapsulating requests
  
- **State Pattern:**
  - Object behavior changes with state
  
- **Composite Pattern:**
  - Tree structures
  
- **Facade Pattern:**
  - Simplified interface to complex system

### Practice Projects:
- Coffee shop with Decorator
- Legacy code integration with Adapter
- Game AI with State pattern
- Undo/Redo with Command pattern
- File system with Composite

---

## Week 21: Refactoring, Quality, and Integration
**CS3330 Week 16 Coverage**

### Topics:
- **Code Smells:**
  - Long methods
  - Large classes
  - Duplicate code
  - Dead code
  
- **Refactoring Techniques:**
  - Extract method
  - Rename
  - Move method/field
  - Extract class
  
- **Code Quality:**
  - Clean code principles
  - Naming conventions
  - Comments and documentation
  - Code review
  
- **Integration:**
  - Version control (Git basics)
  - Continuous integration
  - Build tools (Maven/Gradle basics)
  - Code quality tools

### Practice Projects:
- Refactor legacy code
- Improve code quality metrics
- Set up Git repository
- Create clean, documented project

---

## Phase 2 Summary

By completing this phase, you will have mastered:
- ✅ All Java OOP concepts
- ✅ Design patterns and principles
- ✅ Testing and quality practices
- ✅ GUI development
- ✅ Functional programming
- ✅ Software architecture (MVC)
- ✅ Clean code and refactoring

**Total Duration:** 17 weeks (Weeks 5-21)

---

## Weekly Study Plan Recommendation

**For Each Week:**
1. **Day 1-2:** Read and understand concepts
2. **Day 3-4:** Code examples and small exercises
3. **Day 5-6:** Complete practice project
4. **Day 7:** Review, test yourself, and prepare for next week

**Daily Time Commitment:** 1-2 hours
**Project Time:** 3-4 hours per week

---

## After Phase 2

Once you complete this phase, you will:
- **Excel in CS3330** - You'll know every topic deeply
- **Be ready for advanced courses**
- **Have a strong portfolio** of projects
- **Be prepared for** Phase 3 (Data Structures) and Phase 4 (Algorithms for FAANG)

---

## Integration with Your Class

- Complete our weeks **ahead** of CS3330 topics
- Use CS3330 homework to reinforce learning
- Practice extra on our exercises
- You'll understand lectures deeply because you've already practiced

**Result:** You'll be the TOP student in your CS3330 class! 🏆

---

Ready to dominate CS3330? First, let's finish Phase 1 (Week 4 - Arrays and Strings), then we'll dive deep into Phase 2!