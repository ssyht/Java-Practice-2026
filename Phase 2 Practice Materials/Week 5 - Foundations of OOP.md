# Week 5: Java and OOP Foundations

## What You'll Learn This Week
- What is Object-Oriented Programming (OOP)?
- The difference between a Class and an Object
- Creating your first class
- Instance variables and methods
- The `this` keyword
- Static vs Non-static

---

## What is Object-Oriented Programming?

### The Problem We're Solving

Imagine you're building a student management system. Without OOP, you'd write code like this:

```java
String student1Name = "Alice";
int student1Age = 20;
double student1GPA = 3.8;

String student2Name = "Bob";
int student2Age = 21;
double student2GPA = 3.5;

String student3Name = "Charlie";
int student3Age = 19;
double student3GPA = 3.9;

// This gets messy FAST with 100 students!
```

**Problems:**
- Too many variables
- Hard to organize
- Difficult to manage
- What if we need to add more info? (email, phone, major...)

### The OOP Solution

With OOP, we create a **blueprint** (called a Class) for a Student, then create individual students from that blueprint:

```java
// Create a Student blueprint
class Student {
    String name;
    int age;
    double gpa;
}

// Create individual students
Student student1 = new Student();
student1.name = "Alice";
student1.age = 20;
student1.gpa = 3.8;

Student student2 = new Student();
student2.name = "Bob";
// Much cleaner and organized!
```

---

## Understanding Classes and Objects

### Real-World Analogy

Think of **Class** as a **cookie cutter** and **Objects** as the **cookies**.

- **Class (Cookie Cutter):** The blueprint/template/design
- **Object (Cookie):** The actual thing created from that blueprint

**Another Example:**
- **Class:** Car blueprint (design showing: 4 wheels, engine, seats)
- **Objects:** Your actual car, my car, your friend's car (all made from that blueprint)

### In Java:

**Class = Blueprint**
```java
class Car {
    String color;
    String model;
    int year;
}
```

**Objects = Actual Cars**
```java
Car myCar = new Car();
myCar.color = "Red";
myCar.model = "Tesla";
myCar.year = 2024;

Car yourCar = new Car();
yourCar.color = "Blue";
yourCar.model = "Toyota";
yourCar.year = 2023;
```

**Key Point:** One class, many objects!

---

## Day 1-2: Creating Your First Class

### Example 1: Simple Person Class

Let's create a Person class step by step.

```java
public class Person {
    // These are called INSTANCE VARIABLES (or fields/attributes)
    String name;
    int age;
    String city;
}
```

**Breaking it down:**
- `public class Person` - Creates a class named Person
- `String name;` - Every Person has a name
- `int age;` - Every Person has an age
- `String city;` - Every Person has a city

### Creating Objects (Instances)

Now let's create actual Person objects:

```java
public class PersonDemo {
    public static void main(String[] args) {
        // Create first person
        Person person1 = new Person();
        person1.name = "Alice";
        person1.age = 25;
        person1.city = "New York";
        
        // Create second person
        Person person2 = new Person();
        person2.name = "Bob";
        person2.age = 30;
        person2.city = "Boston";
        
        // Display information
        System.out.println(person1.name + " is " + person1.age + " years old.");
        System.out.println(person2.name + " is " + person2.age + " years old.");
    }
}
```

**Output:**
```
Alice is 25 years old.
Bob is 30 years old.
```

### Understanding the Syntax

```java
Person person1 = new Person();
```

Let's break this down:
1. `Person` - The data type (the class/blueprint)
2. `person1` - The variable name (like naming your car "myCar")
3. `=` - Assignment
4. `new` - Creates a new object in memory
5. `Person()` - Calls the constructor (we'll learn this soon)

**Accessing Variables:**
```java
person1.name = "Alice";  // Set the name
System.out.println(person1.name);  // Get the name
```

Use the **dot (.)** to access variables: `objectName.variableName`

---

## Day 3-4: Adding Methods to Classes

### What are Methods?

Methods are **actions** or **behaviors** that objects can perform.

**Real-World Example:**
- A Dog can: bark(), eat(), sleep()
- A Car can: start(), stop(), accelerate()

### Example: Person with Methods

```java
public class Person {
    // Instance variables
    String name;
    int age;
    String city;
    
    // Methods (actions this person can do)
    
    void introduce() {
        System.out.println("Hi, I'm " + name + " and I'm " + age + " years old.");
    }
    
    void haveBirthday() {
        age++;
        System.out.println("Happy birthday! Now " + age + " years old.");
    }
    
    void move(String newCity) {
        System.out.println("Moving from " + city + " to " + newCity);
        city = newCity;
    }
}
```

### Using Methods

```java
public class PersonDemo {
    public static void main(String[] args) {
        Person person = new Person();
        person.name = "Alice";
        person.age = 25;
        person.city = "New York";
        
        // Call methods
        person.introduce();  // Hi, I'm Alice and I'm 25 years old.
        person.haveBirthday();  // Happy birthday! Now 26 years old.
        person.move("Boston");  // Moving from New York to Boston
        person.introduce();  // Hi, I'm Alice and I'm 26 years old.
    }
}
```

### Methods with Return Values

```java
public class Calculator {
    int number1;
    int number2;
    
    // Method that RETURNS a value
    int add() {
        return number1 + number2;
    }
    
    int multiply() {
        return number1 * number2;
    }
    
    boolean isFirstLarger() {
        return number1 > number2;
    }
}
```

**Using it:**
```java
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        calc.number1 = 10;
        calc.number2 = 5;
        
        int sum = calc.add();
        System.out.println("Sum: " + sum);  // Sum: 15
        
        int product = calc.multiply();
        System.out.println("Product: " + product);  // Product: 50
        
        if (calc.isFirstLarger()) {
            System.out.println("First number is larger");
        }
    }
}
```

### Method Syntax Breakdown

```java
returnType methodName(parameters) {
    // method body
    return value;  // if returnType is not void
}
```

**Examples:**
```java
void sayHello() {          // No return value
    System.out.println("Hello");
}

int getAge() {             // Returns an int
    return age;
}

String getName() {         // Returns a String
    return name;
}

void setAge(int newAge) {  // Takes a parameter
    age = newAge;
}
```

---

## Day 5: The `this` Keyword

### What is `this`?

`this` refers to **the current object**. It's like saying "this specific object I'm working with right now."

### Why Do We Need `this`?

**Problem:** Parameter names conflict with instance variable names

```java
public class Person {
    String name;
    int age;
    
    // Confusing! Which 'name' is which?
    void setName(String name) {
        name = name;  // ❌ This doesn't work!
    }
}
```

**Solution:** Use `this` to distinguish

```java
public class Person {
    String name;
    int age;
    
    void setName(String name) {
        this.name = name;  // ✅ Clear!
        // this.name = instance variable
        // name = parameter
    }
    
    void setAge(int age) {
        this.age = age;
    }
}
```

### Visual Understanding

```java
Person person1 = new Person();
person1.setName("Alice");
// Inside setName: "this" refers to person1

Person person2 = new Person();
person2.setName("Bob");
// Inside setName: "this" refers to person2
```

### Complete Example

```java
public class BankAccount {
    String accountNumber;
    String owner;
    double balance;
    
    void deposit(double amount) {
        this.balance = this.balance + amount;
        System.out.println("Deposited: $" + amount);
        System.out.println("New balance: $" + this.balance);
    }
    
    void withdraw(double amount) {
        if (this.balance >= amount) {
            this.balance = this.balance - amount;
            System.out.println("Withdrew: $" + amount);
        } else {
            System.out.println("Insufficient funds!");
        }
    }
    
    void displayInfo() {
        System.out.println("Account: " + this.accountNumber);
        System.out.println("Owner: " + this.owner);
        System.out.println("Balance: $" + this.balance);
    }
}
```

**Using it:**
```java
public class BankDemo {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        account.accountNumber = "123456";
        account.owner = "Alice";
        account.balance = 1000.0;
        
        account.displayInfo();
        account.deposit(500);
        account.withdraw(200);
        account.displayInfo();
    }
}
```

---

## Day 6-7: Static vs Non-Static (Instance)

### Understanding the Difference

**Instance Variables/Methods:** Each object has its OWN copy
**Static Variables/Methods:** SHARED by ALL objects of that class

### Real-World Analogy

**Instance (Non-Static):**
- Each student has their own name, age, GPA
- Alice's GPA is different from Bob's GPA

**Static:**
- All students go to the same school
- The school name is SHARED by all students

### Code Example

```java
public class Student {
    // Instance variables (each student has their own)
    String name;
    int age;
    double gpa;
    
    // Static variable (shared by ALL students)
    static String schoolName = "ABC University";
    static int totalStudents = 0;
    
    // Instance method (works with specific student)
    void study() {
        System.out.println(name + " is studying.");
    }
    
    // Static method (works with the class, not specific student)
    static void displaySchoolName() {
        System.out.println("School: " + schoolName);
    }
}
```

### Using Static vs Instance

```java
public class StudentDemo {
    public static void main(String[] args) {
        // Access static variable without creating object
        System.out.println(Student.schoolName);  // ABC University
        
        // Call static method without creating object
        Student.displaySchoolName();
        
        // Create students
        Student alice = new Student();
        alice.name = "Alice";
        alice.age = 20;
        
        Student bob = new Student();
        bob.name = "Bob";
        bob.age = 21;
        
        // Instance method (need object)
        alice.study();  // Alice is studying.
        bob.study();    // Bob is studying.
        
        // Both share the same school
        System.out.println(alice.schoolName);  // ABC University
        System.out.println(bob.schoolName);    // ABC University
        
        // If we change static variable
        Student.schoolName = "XYZ University";
        System.out.println(alice.schoolName);  // XYZ University
        System.out.println(bob.schoolName);    // XYZ University
        // Changed for EVERYONE!
    }
}
```

### When to Use Static

**Use Static When:**
- Variable/method belongs to the CLASS, not individual objects
- Utility methods (like Math.sqrt(), Math.max())
- Constants (like Math.PI)
- Counters shared across all objects

**Use Instance (Non-Static) When:**
- Each object needs its own value
- Method needs to access instance variables

### Common Static Examples

```java
// Math class methods are static
int max = Math.max(10, 20);
double squareRoot = Math.sqrt(25);
double pi = Math.PI;

// You don't create a Math object!
// You just use: Math.methodName()
```

### Example: Counter

```java
public class Person {
    String name;
    static int personCount = 0;  // Count total people
    
    void createPerson(String name) {
        this.name = name;
        personCount++;  // Increment for each new person
    }
    
    static void displayCount() {
        System.out.println("Total people: " + personCount);
    }
}
```

```java
Person p1 = new Person();
p1.createPerson("Alice");

Person p2 = new Person();
p2.createPerson("Bob");

Person p3 = new Person();
p3.createPerson("Charlie");

Person.displayCount();  // Total people: 3
```

---

## Complete Example: Car Class

Let's put everything together:

```java
public class Car {
    // Instance variables (each car has its own)
    String brand;
    String model;
    int year;
    String color;
    double speed;
    
    // Static variable (shared by all cars)
    static int totalCars = 0;
    static final int MAX_SPEED = 200;  // Constant
    
    // Constructor (we'll learn more about this next week)
    Car(String brand, String model, int year, String color) {
        this.brand = brand;
        this.model = model;
        this.year = year;
        this.color = color;
        this.speed = 0;
        totalCars++;  // Increment total cars
    }
    
    // Instance methods
    void accelerate(double increment) {
        if (this.speed + increment <= MAX_SPEED) {
            this.speed += increment;
            System.out.println("Speed: " + this.speed + " mph");
        } else {
            System.out.println("Cannot exceed max speed!");
        }
    }
    
    void brake(double decrement) {
        if (this.speed - decrement >= 0) {
            this.speed -= decrement;
            System.out.println("Speed: " + this.speed + " mph");
        } else {
            this.speed = 0;
            System.out.println("Car stopped.");
        }
    }
    
    void displayInfo() {
        System.out.println("\n--- Car Info ---");
        System.out.println("Brand: " + this.brand);
        System.out.println("Model: " + this.model);
        System.out.println("Year: " + this.year);
        System.out.println("Color: " + this.color);
        System.out.println("Speed: " + this.speed + " mph");
    }
    
    // Static method
    static void displayTotalCars() {
        System.out.println("Total cars created: " + totalCars);
    }
}
```

**Using the Car class:**

```java
public class CarDemo {
    public static void main(String[] args) {
        // Create first car
        Car car1 = new Car("Tesla", "Model 3", 2024, "Red");
        car1.displayInfo();
        car1.accelerate(60);
        car1.accelerate(40);
        
        // Create second car
        Car car2 = new Car("Toyota", "Camry", 2023, "Blue");
        car2.displayInfo();
        car2.accelerate(50);
        car2.brake(20);
        
        // Display total cars (static method)
        Car.displayTotalCars();  // Total cars created: 2
    }
}
```

---

## Week 5 Practice Exercises

### Exercise 1: Rectangle Class
Create a `Rectangle` class with:
- Instance variables: `width`, `height`
- Methods:
  - `calculateArea()` - returns width * height
  - `calculatePerimeter()` - returns 2 * (width + height)
  - `displayInfo()` - prints width, height, area, perimeter

Test it with multiple Rectangle objects.

### Exercise 2: Book Class
Create a `Book` class with:
- Instance variables: `title`, `author`, `pages`, `currentPage`
- Static variable: `totalBooks` (count all books created)
- Methods:
  - `read(int pages)` - increases currentPage
  - `getProgress()` - returns percentage of book read
  - `displayInfo()` - prints book information

### Exercise 3: BankAccount Class
Create a `BankAccount` class with:
- Instance variables: `accountNumber`, `owner`, `balance`
- Methods:
  - `deposit(double amount)` - adds to balance
  - `withdraw(double amount)` - subtracts if sufficient funds
  - `transfer(BankAccount other, double amount)` - transfers money to another account
  - `displayBalance()` - shows current balance

Create 2 accounts and test transferring money between them.

### Exercise 4: Student Class
Create a `Student` class with:
- Instance variables: `name`, `id`, `gpa`
- Static variable: `schoolName`, `totalStudents`
- Methods:
  - `updateGPA(double newGPA)` - updates the GPA
  - `displayInfo()` - shows student information
  - Static method: `changeSchool(String newSchool)` - changes school for all students

### Exercise 5: Circle Class
Create a `Circle` class with:
- Instance variable: `radius`
- Static variable: `PI = 3.14159`
- Methods:
  - `calculateArea()` - returns PI * radius * radius
  - `calculateCircumference()` - returns 2 * PI * radius
  - `displayInfo()` - prints radius, area, circumference

---

## Common Mistakes to Avoid

### 1. Forgetting `new` keyword
```java
Person person1;  // ❌ Only declares, doesn't create object
person1.name = "Alice";  // ERROR!

Person person1 = new Person();  // ✅ Creates the object
person1.name = "Alice";  // Works!
```

### 2. Confusing static and instance
```java
public class Test {
    int instanceVar = 10;
    static int staticVar = 20;
    
    static void staticMethod() {
        System.out.println(staticVar);  // ✅ OK
        System.out.println(instanceVar);  // ❌ ERROR! Can't access instance from static
    }
}
```

### 3. Not using `this` when needed
```java
void setName(String name) {
    name = name;  // ❌ Doesn't work - both refer to parameter
    this.name = name;  // ✅ Correct - clear distinction
}
```

### 4. Accessing instance variables from static methods
```java
static void display() {
    System.out.println(name);  // ❌ ERROR! Static can't access instance
}
```

---

## Week 5 Checklist

By the end of this week, you should be able to:
- [ ] Explain what OOP is and why we use it
- [ ] Understand the difference between a class and an object
- [ ] Create classes with instance variables
- [ ] Create and use objects
- [ ] Write and call methods
- [ ] Use the `this` keyword correctly
- [ ] Understand static vs instance (non-static)
- [ ] Complete all 5 practice exercises

---

## Next Week Preview

In Week 6, we'll learn:
- **Constructors** - Special methods to initialize objects
- **Encapsulation** - Hiding data with private/public
- **Getters and Setters** - Controlled access to variables
- **Immutability** - Creating objects that can't be changed

---

## Tips for Success

1. **Think in objects:** Look around you - everything is an object!
2. **Draw diagrams:** Visualize classes and objects on paper
3. **Start simple:** Master basic classes before adding complexity
4. **Practice daily:** Create different classes (Dog, Phone, Game, etc.)
5. **Ask "What attributes and actions?"** for any object you create

---

**Congratulations!** You've taken your first step into Object-Oriented Programming! This is a completely new way of thinking about code, and it's the foundation of modern Java development.

Take your time with the exercises. OOP can feel abstract at first, but with practice, it becomes natural! 🎉

Ready for Week 6? Let me know when you've completed the Week 5 exercises!