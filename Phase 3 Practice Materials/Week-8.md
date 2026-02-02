# Week 8: Inheritance, Interfaces, and Polymorphism

## Overview
- Inheritance and the "Is-a" relationship
- Method overriding
- The `super` keyword
- Abstract classes
- Interfaces
- Polymorphism
- When to use inheritance vs composition

---

## What You'll Learn This Week
- **Inheritance** - Creating subclasses that inherit from parent classes
- **Method Overriding** - Changing parent behavior in subclasses
- **`super` keyword** - Accessing parent class members
- **Abstract Classes** - Templates that cannot be instantiated
- **Interfaces** - Contracts that classes must implement
- **Polymorphism** - Objects of different types treated uniformly
- **Design Principles** - When to inherit vs compose

---

## Day 1: Introduction to Inheritance

### What Is Inheritance?

**Inheritance** = A way to create a new class based on an existing class.

**Real-World Analogy:**
- A **Dog** is a type of **Animal**
- A **Car** is a type of **Vehicle**
- A **Student** is a type of **Person**

The new class (child/subclass) **inherits** all the properties and behaviors of the existing class (parent/superclass).

---

### The "Is-a" Relationship

Use inheritance when there's a clear **"is-a"** relationship:
- A Dog **IS AN** Animal ✅
- A Car **IS A** Vehicle ✅
- A Circle **IS A** Shape ✅

**NOT "is-a":**
- A Car **HAS AN** Engine ❌ (use composition instead)
- A Student **HAS A** Course ❌ (use aggregation instead)

---

### Basic Inheritance Syntax

```java
public class ParentClass {
    // parent stuff
}

public class ChildClass extends ParentClass {
    // child stuff + inherits parent stuff
}
```

**The `extends` keyword creates the inheritance relationship.**

---

### Simple Example: Animal and Dog

```java
// Parent class (Superclass)
public class Animal {
    protected String name;  // protected = accessible in subclasses
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    public void makeSound() {
        System.out.println("Some generic animal sound");
    }
}

// Child class (Subclass)
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // Call parent constructor
        this.breed = breed;
    }
    
    // Dog inherits: name, age, eat(), sleep(), makeSound()
    
    // Dog adds its own method
    public void bark() {
        System.out.println(name + " says: Woof! Woof!");
    }
    
    public void fetch() {
        System.out.println(name + " is fetching the ball!");
    }
}
```

**Using it:**
```java
Dog myDog = new Dog("Buddy", 3, "Golden Retriever");

// Methods from Animal (inherited)
myDog.eat();      // Buddy is eating
myDog.sleep();    // Buddy is sleeping

// Methods from Dog (its own)
myDog.bark();     // Buddy says: Woof! Woof!
myDog.fetch();    // Buddy is fetching the ball!
```

**What Dog Inherits:**
- ✅ `name` and `age` fields (protected)
- ✅ `eat()` method
- ✅ `sleep()` method
- ✅ `makeSound()` method

**What Dog Adds:**
- `breed` field
- `bark()` method
- `fetch()` method

---

### The `super` Keyword

**`super` is used to:**
1. Call the parent constructor
2. Access parent methods
3. Access parent fields

#### 1. Calling Parent Constructor

```java
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // ← Call Animal's constructor
        this.breed = breed;
    }
}
```

**Rules:**
- `super()` must be the **first statement** in constructor
- If you don't call `super()`, Java automatically calls `super()` with no arguments

#### 2. Accessing Parent Methods

```java
public class Dog extends Animal {
    public void displayInfo() {
        super.eat();  // Call parent's eat() method
        bark();       // Call own bark() method
    }
}
```

#### 3. Accessing Parent Fields

```java
public class Dog extends Animal {
    public void introduce() {
        System.out.println("I'm " + super.name);  // Access parent's name
    }
}
```

---

## Day 2: Method Overriding

### What Is Method Overriding?

**Method Overriding** = Changing the behavior of a parent's method in the child class.

The child class provides its own implementation of a method that already exists in the parent.

---

### Example: Overriding makeSound()

```java
public class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public void makeSound() {
        System.out.println("Some generic animal sound");
    }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
    
    @Override  // ← This annotation is optional but recommended
    public void makeSound() {
        System.out.println(name + " says: Woof!");
    }
}

public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " says: Meow!");
    }
}

public class Cow extends Animal {
    public Cow(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " says: Moo!");
    }
}
```

**Using it:**
```java
Dog dog = new Dog("Buddy");
Cat cat = new Cat("Whiskers");
Cow cow = new Cow("Bessie");

dog.makeSound();  // Buddy says: Woof!
cat.makeSound();  // Whiskers says: Meow!
cow.makeSound();  // Bessie says: Moo!
```

**Each subclass overrides `makeSound()` with its own implementation!**

---

### The @Override Annotation

```java
@Override
public void makeSound() {
    // ...
}
```

**Benefits:**
- Tells the compiler you're intentionally overriding
- Compiler checks that the method actually exists in parent
- Catches typos and mistakes

**Example of catching mistakes:**
```java
public class Dog extends Animal {
    @Override
    public void makesound() {  // ← lowercase 's' - typo!
        // Compiler ERROR: Method does not override method from superclass
    }
}
```

Without `@Override`, this would create a NEW method instead of overriding, and you wouldn't get an error!

---

### Rules for Method Overriding

1. **Same method signature** (name, parameters, return type)
2. **Cannot be more restrictive** in access level
3. **Can be less restrictive** in access level

```java
public class Animal {
    public void move() { }      // public
    protected void eat() { }    // protected
    void sleep() { }            // package-private
}

public class Dog extends Animal {
    @Override
    public void move() { }      // ✅ OK (same access level)
    
    @Override
    public void eat() { }       // ✅ OK (less restrictive: protected → public)
    
    @Override
    private void sleep() { }    // ❌ ERROR (more restrictive: package → private)
}
```

---

### Calling Parent's Overridden Method

Sometimes you want to **extend** the parent's behavior, not completely replace it:

```java
public class Animal {
    protected String name;
    
    public void eat() {
        System.out.println(name + " is eating");
    }
}

public class Dog extends Animal {
    @Override
    public void eat() {
        super.eat();  // Call parent's eat() first
        System.out.println(name + " is wagging tail while eating!");
    }
}
```

**Using it:**
```java
Dog dog = new Dog("Buddy");
dog.eat();
// Output:
// Buddy is eating
// Buddy is wagging tail while eating!
```

---

## Day 3: Abstract Classes

### What Is an Abstract Class?

**Abstract Class** = A class that cannot be instantiated directly. It serves as a template for subclasses.

**Use when:**
- You want to define common behavior for subclasses
- Some methods should be implemented by subclasses
- You want to prevent direct instantiation

---

### Abstract Class Syntax

```java
public abstract class ClassName {
    // Can have regular methods
    public void regularMethod() {
        // implementation
    }
    
    // Can have abstract methods (no implementation)
    public abstract void abstractMethod();
}
```

**Key Points:**
- Use `abstract` keyword for the class
- Can have both abstract and concrete (regular) methods
- **Cannot create objects** of an abstract class
- Subclasses **must** implement all abstract methods

---

### Example: Shape Hierarchy

```java
// Abstract parent class
public abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    // Regular method (all shapes have this)
    public void displayColor() {
        System.out.println("Color: " + color);
    }
    
    // Abstract methods (each shape calculates differently)
    public abstract double getArea();
    public abstract double getPerimeter();
}

// Concrete subclass 1
public class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }
}

// Concrete subclass 2
public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double getArea() {
        return width * height;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * (width + height);
    }
}
```

**Using it:**
```java
// Shape shape = new Shape("Red");  // ❌ ERROR! Cannot instantiate abstract class

Circle circle = new Circle("Red", 5);
Rectangle rect = new Rectangle("Blue", 4, 6);

System.out.println("Circle area: " + circle.getArea());
System.out.println("Rectangle area: " + rect.getArea());

circle.displayColor();  // Inherited from Shape
```

**Why use abstract?**
- Every shape needs `getArea()` and `getPerimeter()`
- But each shape calculates them differently
- Abstract class forces subclasses to implement these methods
- Provides common functionality in `displayColor()`

---

## Day 4: Interfaces

### What Is an Interface?

**Interface** = A contract that specifies what methods a class must have, but not how to implement them.

**Think of it as:** A pure contract with NO implementation.

---

### Interface Syntax

```java
public interface InterfaceName {
    // All methods are public and abstract by default
    void method1();
    void method2();
    int method3(String param);
}
```

**Key Points:**
- All methods are `public abstract` by default
- No constructors
- Can have constants (public static final)
- A class **implements** an interface
- A class can implement **multiple interfaces** (unlike inheritance!)

---

### Example: Drawable Interface

```java
// Define the interface
public interface Drawable {
    void draw();  // Any class that implements Drawable MUST have draw()
}

// Class 1 implements Drawable
public class Circle implements Drawable {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a circle with radius " + radius);
    }
}

// Class 2 implements Drawable
public class Rectangle implements Drawable {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle " + width + "x" + height);
    }
}
```

**Using it:**
```java
Circle circle = new Circle(5);
Rectangle rect = new Rectangle(4, 6);

circle.draw();  // Drawing a circle with radius 5.0
rect.draw();    // Drawing a rectangle 4.0x6.0
```

---

### Multiple Interfaces

A class can implement multiple interfaces:

```java
public interface Drawable {
    void draw();
}

public interface Movable {
    void move(int x, int y);
}

// Class implements BOTH interfaces
public class Player implements Drawable, Movable {
    private int x, y;
    
    @Override
    public void draw() {
        System.out.println("Drawing player at (" + x + ", " + y + ")");
    }
    
    @Override
    public void move(int newX, int newY) {
        this.x = newX;
        this.y = newY;
        System.out.println("Player moved to (" + x + ", " + y + ")");
    }
}
```

---

### Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Keyword | `extends` | `implements` |
| Multiple? | NO (single inheritance) | YES (multiple interfaces) |
| Constructor | YES | NO |
| Fields | Any (private, protected, public) | Only public static final |
| Methods | Abstract + Concrete | Only abstract (before Java 8) |
| When to use | "Is-a" relationship, shared code | "Can-do" contract, no shared code |

**General Rule:**
- Use **abstract class** when subclasses share common code
- Use **interface** when you just need a contract/capability

---

## Day 5: Polymorphism

### What Is Polymorphism?

**Polymorphism** = "Many forms" - the ability to treat objects of different types uniformly.

**Real-World Example:**
- A **remote control** (interface) works with many devices (TV, stereo, DVD player)
- You press "play" and each device does something different
- Same interface, different behavior!

---

### Polymorphism with Inheritance

```java
public class Animal {
    public void makeSound() {
        System.out.println("Some sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}

public class Bird extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Chirp!");
    }
}
```

**Polymorphism in action:**
```java
Animal[] animals = new Animal[3];
animals[0] = new Dog();
animals[1] = new Cat();
animals[2] = new Bird();

for (Animal animal : animals) {
    animal.makeSound();  // Different behavior for each!
}

// Output:
// Woof!
// Meow!
// Chirp!
```

**Key Point:** We treat all animals the same way (call `makeSound()`), but each behaves differently!

---

### Polymorphism with Interfaces

```java
public interface Playable {
    void play();
}

public class VideoGame implements Playable {
    @Override
    public void play() {
        System.out.println("Playing video game");
    }
}

public class BoardGame implements Playable {
    @Override
    public void play() {
        System.out.println("Playing board game");
    }
}

public class MusicPlayer implements Playable {
    @Override
    public void play() {
        System.out.println("Playing music");
    }
}
```

**Using polymorphism:**
```java
Playable[] items = new Playable[3];
items[0] = new VideoGame();
items[1] = new BoardGame();
items[2] = new MusicPlayer();

for (Playable item : items) {
    item.play();  // Each plays differently!
}
```

---

### Benefits of Polymorphism

1. **Flexibility** - Add new types without changing existing code
2. **Maintainability** - Common interface, different implementations
3. **Extensibility** - Easy to add new subclasses

**Example: Adding a new animal**
```java
public class Lion extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Roar!");
    }
}

// Can use it with existing code - no changes needed!
Animal animal = new Lion();
animal.makeSound();  // Roar!
```

---

## Day 6: Inheritance vs Composition

### When to Use Inheritance

✅ **Use inheritance when:**
- Clear "IS-A" relationship exists
- Subclass is a specialized version of parent
- Subclass needs most/all of parent's behavior

**Good examples:**
- Dog IS-A Animal ✅
- Circle IS-A Shape ✅
- Student IS-A Person ✅

### When to Use Composition

✅ **Use composition when:**
- "HAS-A" relationship exists
- You need flexibility to change behavior
- You want to avoid tight coupling

**Good examples:**
- Car HAS-AN Engine ✅
- Person HAS-A Address ✅
- Order HAS OrderItems ✅

---

### The Problem with Inheritance

**Inheritance can be overused!**

```java
// ❌ BAD - Overusing inheritance
public class Person {
    String name;
}

public class Employee extends Person {
    int employeeId;
}

public class Manager extends Employee {
    String department;
}

public class ProjectManager extends Manager {
    String projectName;
}

// This creates a rigid, deep hierarchy!
```

**Problems:**
- Hard to change
- Tight coupling
- Hard to reuse code

---

### Prefer Composition

```java
// ✅ BETTER - Using composition
public class Address {
    private String street;
    private String city;
    // ...
}

public class ContactInfo {
    private String email;
    private String phone;
    // ...
}

public class Person {
    private String name;
    private Address address;      // HAS-A
    private ContactInfo contact;  // HAS-A
    
    // Much more flexible!
}
```

---

### The "Favor Composition Over Inheritance" Principle

**General guideline:**
1. Start with composition
2. Only use inheritance if there's a clear "IS-A" relationship
3. Keep inheritance hierarchies shallow (2-3 levels max)

---

## Week 8 Practice Exercises

### Exercise 1: Vehicle Hierarchy

Create a vehicle system using inheritance:

**Classes:**
- `Vehicle` (abstract)
  - fields: make, model, year
  - abstract methods: startEngine(), stopEngine()
  - regular method: displayInfo()
- `Car extends Vehicle`
  - additional field: numDoors
  - implement abstract methods
- `Motorcycle extends Vehicle`
  - additional field: hasSidecar
  - implement abstract methods

---

### Exercise 2: Employee System

Create an employee payroll system:

**Classes:**
- `Employee` (abstract)
  - fields: name, id
  - abstract method: calculatePay()
  - regular method: displayInfo()
- `HourlyEmployee extends Employee`
  - fields: hourlyRate, hoursWorked
  - implement calculatePay()
- `SalariedEmployee extends Employee`
  - fields: annualSalary
  - implement calculatePay()

---

### Exercise 3: Interface Practice

Create multiple interfaces:

**Interfaces:**
- `Flyable` (method: fly())
- `Swimmable` (method: swim())

**Classes:**
- `Duck implements Flyable, Swimmable`
- `Airplane implements Flyable`
- `Fish implements Swimmable`

---

### Exercise 4: Polymorphism with Shapes

Create a shape system demonstrating polymorphism:

**Classes:**
- `Shape` (abstract with abstract methods getArea(), getPerimeter())
- `Circle extends Shape`
- `Rectangle extends Shape`
- `Triangle extends Shape`

**Test:**
- Create an array of shapes
- Loop through and display area/perimeter of each
- Demonstrate polymorphism

---

### Exercise 5: Complete System

Create a zoo management system:

**Classes:**
- `Animal` (abstract)
- `Mammal extends Animal` (abstract)
- `Bird extends Animal` (abstract)
- `Lion extends Mammal`
- `Eagle extends Bird`
- `Zoo` (has Animal array)

**Include:**
- Method overriding
- Polymorphism
- Abstract methods
- Proper use of super

---

## Common Mistakes

### Mistake 1: Forgetting to Call super()

```java
// ❌ WRONG
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        // Forgot super(name, age)!
        this.breed = breed;
    }
}

// ✅ CORRECT
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // Must call parent constructor
        this.breed = breed;
    }
}
```

### Mistake 2: Overusing Inheritance

```java
// ❌ BAD - No clear IS-A relationship
public class Stack extends ArrayList {
    // Stack IS-NOT-A ArrayList!
    // Should use composition instead
}

// ✅ BETTER
public class Stack {
    private ArrayList items;  // HAS-A
    // ...
}
```

### Mistake 3: Not Using @Override

```java
// ❌ BAD - Typo creates new method instead of overriding
public class Dog extends Animal {
    public void makesound() {  // Lowercase 's' - typo!
        System.out.println("Woof");
    }
}

// ✅ GOOD - Compiler catches the error
public class Dog extends Animal {
    @Override
    public void makesound() {  // Compiler ERROR!
        System.out.println("Woof");
    }
}
```

---

## Week 8 Checklist

By the end of this week, you should be able to:
- [ ] Create subclasses using `extends`
- [ ] Override methods with `@Override`
- [ ] Use `super` to access parent members
- [ ] Create and use abstract classes
- [ ] Define and implement interfaces
- [ ] Demonstrate polymorphism
- [ ] Decide when to use inheritance vs composition
- [ ] Complete all 5 practice exercises

---

## Key Takeaways

### Inheritance:
- **"IS-A"** relationship
- Use `extends` keyword
- Subclass inherits parent's members
- Use `super` to access parent

### Method Overriding:
- Change parent's method behavior
- Use `@Override` annotation
- Same signature, different implementation

### Abstract Classes:
- Cannot be instantiated
- Can have abstract and concrete methods
- Use when sharing common code

### Interfaces:
- Pure contracts
- All methods abstract (before Java 8)
- Class can implement multiple
- Use for capabilities/behaviors

### Polymorphism:
- "Many forms"
- Treat different types uniformly
- Enables flexibility and extensibility

### Design:
- **Favor composition over inheritance**
- Keep hierarchies shallow
- Use inheritance for clear IS-A relationships

---

## Next Steps

You've now completed the core OOP concepts! Next topics will include:
- Exception handling
- Collections (ArrayList, HashMap)
- File I/O
- More advanced design patterns

---

Congratulations! You've mastered inheritance, interfaces, and polymorphism - some of the most powerful features of OOP! 🚀